## Ceph Safe Shutdown
---
I kept running into OSD issues because I was just typing reboot on the proxmox CLI. Turns out I need to shut down ceph properly.

###  1. Create the script

Run this on each Ceph node:

```bash
sudo nano /usr/local/sbin/ceph-safe-reboot
```

Paste this:

```bash
#!/bin/bash
# ceph-safe-reboot
# Graceful Ceph reboot helper for Proxmox + Ceph clusters
# Author: HartDevOps

LOGFILE="/var/log/ceph-safe-reboot.log"
echo "---- $(date) Starting Ceph Safe Reboot ----" | tee -a $LOGFILE

# Verify ceph command exists
if ! command -v ceph &>/dev/null; then
    echo "ERROR: ceph command not found. Aborting." | tee -a $LOGFILE
    exit 1
fi

# 1️ Set noout flag
echo "Setting noout flag..." | tee -a $LOGFILE
ceph osd set noout | tee -a $LOGFILE

# 2️ Display current cluster health
echo "Cluster status before shutdown:" | tee -a $LOGFILE
ceph -s | tee -a $LOGFILE

# 3️ Stop Ceph services gracefully
echo "Stopping Ceph services..." | tee -a $LOGFILE
systemctl stop ceph.target
sleep 5
systemctl stop ceph-osd.target ceph-mon.target ceph-mgr.target ceph-mds.target ceph-crash.target 2>/dev/null

# 4️ Verify services stopped
echo "Checking Ceph service status..." | tee -a $LOGFILE
systemctl --no-pager --state=running | grep ceph | tee -a $LOGFILE

if systemctl --no-pager --state=running | grep -q ceph; then
    echo "Warning: Some Ceph services are still running, waiting 10s..." | tee -a $LOGFILE
    sleep 10
    systemctl stop ceph.target
fi

# 5️ Sync disks and flush caches
echo "Syncing disks..." | tee -a $LOGFILE
sync

# 6️ Log and reboot
echo "Ceph services stopped. Rebooting now." | tee -a $LOGFILE
sleep 3
reboot
```

---

###  2. Make it executable

```bash
sudo chmod +x /usr/local/sbin/ceph-safe-reboot
```

---

###  3. Usage

Any time you want to reboot a Ceph node:

```bash
sudo ceph-safe-reboot
```

You’ll see output like:

```
---- Fri Oct 25 10:14:33 2025 Starting Ceph Safe Reboot ----
Setting noout flag...
cluster status before shutdown:
  health: HEALTH_OK
Stopping Ceph services...
Syncing disks...
Ceph services stopped. Rebooting now.
```

And it logs every run to:

```
/var/log/ceph-safe-reboot.log
```

---

# Ceph Storage Overview

## Architecture
- 8 OSDs across 4 nodes  
- 4+2 erasure-coded pool for capacity
- NVMe pool for metadata and WAL/DB

## Key Features
- CephFS for Kubernetes PVCs
- RBD images for VM disks
- Snapshot mirroring + replication

## Performance Tuning
- OSDs pinned to dedicated NVMe WAL/DB
- PG count: 256 per pool (tuned for 4+2)
- Monitors distributed evenly across nodes

## Monitoring
- Dashboards via Prometheus + Ceph Exporter
- Alerts integrated with Alertmanager




