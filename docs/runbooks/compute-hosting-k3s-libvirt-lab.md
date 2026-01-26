# Runbook — Bootstrap Domain 3 lab (libvirt + Debian 12 + k3s + backup/restore)

## Status
Draft

## Owner
Compute & Hosting

## Last reviewed
2026-01-24

---

## Purpose
Build a small but representative Compute & Hosting lab on a single Ubuntu 22.04 laptop using virt-manager/libvirt and Debian 12 VMs.

The lab validates:
- a clear control plane vs workload plane boundary;
- portable workloads (images + configuration + data);
- backups that are proven via restore tests.

## When to use
- You want to validate Domain 3 in practice before running Domain 2 workloads.
- You want a repeatable baseline lab you can rebuild and test restore procedures against.

## Preconditions
- Host: Ubuntu 22.04 with KVM/libvirt + virt-manager
- CPU virtualization enabled (VT-x/AMD-V)
- RAM: 32GB is sufficient (SSD recommended)
- Disk: at least ~150–250GB free for VM disks + images + backup data
- A Debian 12 base VM image (cloud-init or a clonable template)

## Risks
- Time drift breaks tokens/certs and causes confusing failures (ensure NTP/chrony).
- Using one flat network erodes the control/workload boundary.
- Backups that are not restore-tested are a false sense of safety.

## Coexisting with an existing Domain 1/2 lab

Domain 1/2 services are already running on “legacy” standalone VMs.
Domain 3 wil be built **in parallel** as a reference substrate without disrupting existing PoCs.

Rules of thumb:
- Treat the new cluster as a separate environment (e.g. `lab-domain3`) with its own CIDRs.
- Ensure CIDRs do **not overlap** with existing libvirt networks.
- Prefer stable **hostnames and roles** in docs; avoid hard-coding IPs.
- If you later migrate services into the cluster, do it incrementally (one workload at a time).

Recommended naming:
- Existing environment: `lab-legacy`
- New substrate environment: `lab-domain3`

Recommended documentation discipline:
- Keep an inventory note with: environment name, CIDRs, VM names, and the access path.
- If you must reference IPs, always qualify them by environment and network (mgmt vs workload).

---

## Inputs
- Environment: lab
- Host OS: Ubuntu 22.04
- VM base image: Debian 12
- Planned addressing (example below)

## Evidence to collect (before changes)
- VM inventory: names, CPU/RAM/disk
- Network plan: mgmt/workload CIDRs
- Current host virtualization status (KVM available)

---

## Procedure

### 1) Create two libvirt networks (management + workload)

Goal: make the boundary visible and enforceable.

- `tn-mgmt` (NAT) — example CIDR `192.168.56.0/24`
- `tn-work` (isolated) — example CIDR `10.10.10.0/24`

Expected outcome:
- VMs have two NICs (mgmt + work).
- Only the management host/VM is used as the operational entry point.

Verification:
- Each VM has IP connectivity on both networks.

### 2) Create VMs from the Debian 12 template

Recommended VMs:
- `tn-mgmt` (jumpbox)
- `tn-k3s-server`
- `tn-k3s-agent1`
- `tn-k3s-agent2`
- `tn-backup`

Recommended sizing (adjust to your laptop):
- `tn-mgmt`: 2 vCPU, 3–4GB RAM, 20GB disk
- `tn-k3s-server`: 2 vCPU, 6GB RAM, 30GB disk
- `tn-k3s-agent1`: 2 vCPU, 4GB RAM, 30GB disk
- `tn-k3s-agent2`: 2 vCPU, 4GB RAM, 30GB disk
- `tn-backup`: 2 vCPU, 4GB RAM, 80–150GB disk

NICs:
- All: attach both `tn-mgmt` and `tn-work`

OS baseline on all VMs:
- Install `qemu-guest-agent`
- Install and enable `chrony`
- Install `curl` (not always present on minimal Debian 12)
- Disable swap (Kubernetes requirement)

### 3) Assign stable IPs on the workload network

Important (routing + DNS on dual-NIC Debian 12):
- Keep the **default route + DNS** on the management NIC (so installs can reach the internet and resolve names).
- Keep the workload NIC **static** and avoid setting DNS or a gateway there (keeps the boundary clean and avoids confusing routing/DNS issues).

Example (if your Debian 12 template uses ifupdown at `/etc/network/interfaces`):

```text
# Management network (default route + DNS)
allow-hotplug enp1s0
iface enp1s0 inet dhcp
  dns-nameservers 1.1.1.1 8.8.8.8
  metric 100

# Workload network (NO DNS, NO gateway)
auto enp7s0
iface enp7s0 inet static
  address 10.10.10.13
  netmask 255.255.255.0
  metric 200
```

Example plan:
- `tn-mgmt`: `10.10.10.10`
- `tn-k3s-server`: `10.10.10.11`
- `tn-k3s-agent1`: `10.10.10.12`
- `tn-k3s-agent2`: `10.10.10.13`
- `tn-backup`: `10.10.10.20`

Expected outcome:
- k3s uses the workload NIC for cluster traffic.

### 4) Install k3s server

On `tn-k3s-server`:

```bash
sudo apt update
sudo apt install -y curl

curl -sfL https://get.k3s.io | sudo sh -s - server \
  --node-ip 10.10.10.11 \
  --advertise-address 10.10.10.11

sudo cat /var/lib/rancher/k3s/server/node-token
```

Expected outcome:
- API server is reachable at `https://10.10.10.11:6443` from `tn-mgmt`.

### 5) Join agents

On each agent, set node IP to the workload NIC:

```bash
sudo apt update
sudo apt install -y curl

curl -sfL https://get.k3s.io | \
  K3S_URL=https://10.10.10.11:6443 \
  K3S_TOKEN=<TOKEN_FROM_SERVER> \
  sudo sh -s - agent --node-ip 10.10.10.12
```

Repeat for agent2 with `--node-ip 10.10.10.13`.

Verify from `tn-mgmt`:

```bash
kubectl get nodes -o wide
kubectl get pods -A
```

Pitfall (agent join): if `k3s-agent` does not start and `/etc/systemd/system/k3s-agent.service.env` is empty, the install did not persist `K3S_URL`/`K3S_TOKEN`.

Fix on the agent:

```bash
sudo tee /etc/systemd/system/k3s-agent.service.env >/dev/null <<'EOF'
K3S_URL=https://10.10.10.11:6443
K3S_TOKEN=<TOKEN_FROM_SERVER>
EOF

sudo systemctl daemon-reload
sudo systemctl restart k3s-agent

sudo systemctl status k3s-agent --no-pager -l
sudo journalctl -u k3s-agent -b --no-pager | tail -n 200
```

Preferred fix (more repeatable): rerun the installer command above with `K3S_URL=...` and `K3S_TOKEN=...` set inline so the service env file is generated correctly.

### 6) Provide dynamic PVs via NFS (simple, boring, portable)

On `tn-backup`:

```bash
sudo apt update
sudo apt install -y nfs-kernel-server
sudo mkdir -p /srv/nfs/k3s

echo "/srv/nfs/k3s 10.10.10.0/24(rw,sync,no_subtree_check,no_root_squash)" | sudo tee -a /etc/exports
sudo exportfs -ra
```

On `tn-mgmt` (Helm example):

```bash
sudo apt update
sudo apt install -y curl

curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4
chmod 700 get_helm.sh
sudo ./get_helm.sh

helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
helm repo update

helm install nfs nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
  --set nfs.server=10.10.10.20 \
  --set nfs.path=/srv/nfs/k3s
```

Expected outcome:
- A StorageClass exists and can provision PVCs.

Verify:

```bash
kubectl get storageclass
```

### 7) Set up backup target (MinIO) + Velero

This PoC intentionally keeps it generic. The key requirement:
- an S3-compatible endpoint (MinIO) reachable from the cluster;
- Velero configured to store backups there;
- PVC backup enabled (node-agent/restic is simplest).

Validation checks:

```bash
velero backup-location get
velero schedule get
```

### 8) Prove backups via a restore test (workload with real data)

Apply this minimal workload (writes a file to a PVC and serves it):

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: tn-restore-test
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: web-data
  namespace: tn-restore-test
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
  namespace: tn-restore-test
spec:
  replicas: 1
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      initContainers:
        - name: seed
          image: alpine:3.20
          command: ["/bin/sh","-c"]
          args:
            - |
              set -eu
              date -Is > /data/proof.txt
              echo "ok" > /data/health.txt
          volumeMounts:
            - name: data
              mountPath: /data
      containers:
        - name: nginx
          image: nginx:1.27-alpine
          ports:
            - containerPort: 80
          volumeMounts:
            - name: data
              mountPath: /usr/share/nginx/html
      volumes:
        - name: data
          persistentVolumeClaim:
            claimName: web-data
```

Steps:

1. Deploy it:

```bash
kubectl apply -f <file.yaml>
kubectl -n tn-restore-test rollout status deploy/web
```

2. Confirm data exists:

```bash
kubectl -n tn-restore-test exec deploy/web -- cat /usr/share/nginx/html/proof.txt
```

3. Backup + delete:

```bash
velero backup create tn-restore-test-1 --include-namespaces tn-restore-test
velero backup describe tn-restore-test-1

kubectl delete ns tn-restore-test
```

4. Restore + verify:

```bash
velero restore create --from-backup tn-restore-test-1
velero restore get

kubectl -n tn-restore-test rollout status deploy/web
kubectl -n tn-restore-test exec deploy/web -- cat /usr/share/nginx/html/proof.txt
```

Expected outcome:
- Namespace returns.
- Deployment becomes Ready.
- `proof.txt` is restored and contains a timestamp.

---

## Validation (after changes)
- `kubectl get nodes` shows all nodes Ready
- PVC provisioning works
- A Velero backup completes successfully
- A restore recreates the workload and the persisted file

## Rollback
- Destroy the lab VMs and networks.
- Rebuild from the Debian 12 template.

## Communication
- Record the exact VM sizes, CIDRs, and outcomes in your lab notes.

## Follow-ups
- Add a second restore test that restores into a different namespace.
- Add a small failure test (power off one agent, confirm workload reschedules).
- Decide which parts move to Domain 5 (automation as code).

## References
- Domain 3 architecture: `poc/3. Compute & Hosting/architecture.md`
- Domain 3 goal: `poc/3. Compute & Hosting/goal.md`
- Runbook template: `docs/runbook-template.md`
