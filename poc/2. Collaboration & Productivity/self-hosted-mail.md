# Self-hosted mail

This document captures a focused Proof of Concept in the Collaboration & Productivity domain:

- A minimal Postfix + Dovecot mail setup for local users.
- A policy model where LDAP drives mail-sending authorization, and Postfix enforces it at SMTP time.

The intent is architectural clarity and reproducibility, not a production-ready mail platform.

---

## Why this exists

Email is a first-class citizen in the workspace domain, but mail infrastructure must not become an identity authority.

This PoC explores a clean separation:

- LDAP (or the Identity domain) owns user lifecycle and user state.
- Postfix and Dovecot provide mail transport and access.
- Authorization decisions are evaluated dynamically, without service-specific user management.

---

## Problem statement and goal

Postfix does not natively understand user lifecycle state (active, disabled, offboarded). LDAP does.

Goal: enforce mail-sending permissions dynamically, without:

- restarting or reloading Postfix for policy changes;
- modifying mailboxes to express policy;
- breaking authentication flows (SASL) just to enforce authorization.

---

## Core insight

LDAP acts as the policy engine; Postfix acts as the policy enforcer.

User intent and state live in LDAP. Postfix queries LDAP during the SMTP transaction and decides whether a message may be sent.

---

## Policy enforcement in Postfix (LDAP-driven sending control)

### Key mechanism

Mail sending is controlled via a single sender restriction:

`check_sender_access ldap:/etc/postfix/ldap_sender_reject_disabled.cf`

Policy model:

- LDAP stores explicit mail attributes (e.g., `tnMailSendAllowed`, `tnAccountStatus`).
  (Some labs may use non-namespaced equivalents like `mailSendAllowed` / `mailAccountStatus`.)
- If the LDAP lookup matches “send disabled”, Postfix issues a deterministic SMTP reject.
- If the LDAP lookup returns no match, sending is permitted.
- Policy changes in LDAP take effect immediately for new SMTP transactions.

Note: the **definition** and **management** of these attributes (schema, LDIF examples, ownership) belongs in the Identity domain:
- `poc/1. Identity & Access/directory-policy-attributes.md`

### Authentication vs authorization

Authentication (SASL login) is not authorization (permission to send).

This PoC explicitly supports:

- Users can still authenticate successfully.
- Users can still receive mail.
- Users can be blocked from sending mail.

This separation enables clean offboarding and IAM-style control.

### Anti-spoofing

To bind identity to address ownership, enforce:

`reject_sender_login_mismatch`

This prevents authenticated users from sending as another address.

### Configuration discipline

- `/etc/postfix/main.cf` contains policy and logic (restrictions, maps, lookups).
- `/etc/postfix/master.cf` contains service behavior only (ports, TLS, SASL enablement).

Avoid implementing policy logic in `master.cf`; it becomes brittle and difficult to debug.

### What this PoC avoids

- No mailbox deletion or modification.
- No OS-level user disabling.
- No per-service policy overrides.

Policy changes are made in LDAP and enforced by Postfix at the SMTP boundary.

![LDAP-driven mail sending control flow](image.png)

### Verifiable outcome

- Toggling a single LDAP attribute (e.g., `tnMailSendAllowed`) enables/disables sending immediately.
- Blocked sends return consistent SMTP status codes (e.g., `554 5.7.1`).
- Behavior is reproducible (e.g., with `swaks`).

---

## Minimal mailserver setup (Debian 12: Postfix + Dovecot)

Objective: run a local mail system for example users like `alice@truenorth.local` and `bob@truenorth.local`.

### Components

| Component | Role | Notes |
|---|---|---|
| Debian 12 | OS base | Stable, minimal, supported |
| Postfix | SMTP server | Receives/sends mail; enforces SMTP policy |
| Dovecot | IMAP server + SASL | Mailbox access and SMTP AUTH via SASL |
| Thunderbird (optional) | Client | Standards-based IMAP/SMTP |

### Prerequisites

- A Debian 12 host (example: `mailguard.truenorth.local`).
- Static IP (example: `192.168.122.96`).
- Hostname resolvable locally (e.g., `/etc/hosts` on clients and server).
- Local test users (example: `alice`, `bob`) with home dirs in `/home`.

### 1) Install packages

```bash
sudo apt update
sudo apt install postfix postfix-ldap dovecot-core dovecot-imapd swaks
```

### 2) Configure Postfix

Edit `/etc/postfix/main.cf` (example values):

```conf
myhostname = mailguard.truenorth.local
mydomain = truenorth.local
myorigin = $mydomain

inet_interfaces = all
mynetworks = 127.0.0.0/8 [::1]/128 192.168.122.0/24

smtpd_banner = $myhostname ESMTP
append_dot_mydomain = no

compatibility_level = 3.6

# TLS (PoC defaults)
smtpd_use_tls = yes
smtpd_tls_security_level = may
smtpd_tls_cert_file = /etc/ssl/certs/ssl-cert-snakeoil.pem
smtpd_tls_key_file = /etc/ssl/private/ssl-cert-snakeoil.key

# SASL via Dovecot
smtpd_sasl_type = dovecot
smtpd_sasl_path = private/auth
smtpd_sasl_auth_enable = yes
smtpd_sasl_security_options = noanonymous
broken_sasl_auth_clients = yes

# Local delivery
home_mailbox = Maildir/
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain

# Recipient policy
smtpd_recipient_restrictions =
    permit_sasl_authenticated,
    permit_mynetworks,
    reject_unauth_destination

# Sender identity binding + authorization
smtpd_sender_login_maps = ldap:/etc/postfix/ldap_sender_login_maps.cf
smtpd_sender_restrictions =
    reject_sender_login_mismatch,
    check_sender_access ldap:/etc/postfix/ldap_sender_reject_disabled.cf
```

Edit `/etc/postfix/master.cf` and enable submission (587) (example):

```conf
submission inet n       -       y       -       -       smtpd
  -o syslog_name=postfix/submission
  -o smtpd_tls_security_level=may
  -o smtpd_sasl_auth_enable=yes
  -o smtpd_recipient_restrictions=permit_sasl_authenticated,reject
  -o milter_macro_daemon_name=ORIGINATING
```

Restart:

```bash
sudo systemctl restart postfix
```

Note: the LDAP map files referenced above are environment-specific. This PoC assumes they exist and implement (1) sender-login ownership and (2) send-disabled matching.

### 3) Configure Dovecot

Edit `/etc/dovecot/conf.d/10-auth.conf`:

```conf
disable_plaintext_auth = no
auth_mechanisms = plain login
```

Edit `/etc/dovecot/conf.d/10-mail.conf`:

```conf
mail_location = maildir:~/Maildir
```

Edit `/etc/dovecot/conf.d/10-master.conf` (SASL socket for Postfix):

```conf
service auth {
  unix_listener /var/spool/postfix/private/auth {
    mode = 0660
    user = postfix
    group = postfix
  }
}
```

Restart:

```bash
sudo systemctl restart dovecot
```

### 4) Create Maildirs for local users

On Debian, `maildirmake.dovecot` is commonly available with Dovecot:

```bash
sudo -u alice maildirmake.dovecot /home/alice/Maildir
sudo -u bob maildirmake.dovecot /home/bob/Maildir
sudo chmod -R 700 /home/alice/Maildir /home/bob/Maildir
```

### 5) Send a test mail (SMTP AUTH)

```bash
swaks \
  --to bob@truenorth.local \
  --from alice@truenorth.local \
  --server 127.0.0.1 \
  --auth LOGIN \
  --auth-user alice \
  --auth-password 'verystrongpassword'
```

Expected: `250 2.0.0 Ok: queued as ...`.

### 6) Verify delivery

```bash
sudo -u bob ls -la /home/bob/Maildir/new/
```

---

## Debugging

| Issue | Command |
|---|---|
| SMTP connection refused | `sudo ss -tulnp | grep ':25\b'` |
| Auth failures | `sudo tail -n 50 /var/log/mail.log | grep -i sasl` |
| Mail stuck in queue | `sudo mailq` (flush with `sudo postfix flush`) |
| IMAP index issues | `sudo -u alice rm -f /home/alice/Maildir/dovecot-index*` |

---

## Production notes (not implemented here)

- TLS: use real certificates and enforce TLS for IMAP/SMTP.
- Anti-spam: add a filter layer (e.g., rspamd).
- Backups: back up Maildirs and configuration.
- Firewall: restrict 25/143/587 to trusted networks where appropriate.





