 Goal
-------

You'll end up with:

-   A **Proxmox VM template** called `ubuntu-24.04-cloudinit`

-   Based on the official Ubuntu Cloud image

-   Ready for Terraform (with `ciuser`, `ssh_keys`, and networking handled automatically)

* * * * *

Step 1 -- Download the official cloud image
---------------------------------------------

SSH into your Proxmox node and run:

```
cd /var/lib/vz/template/iso
wget https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.img

```

>  *For Ubuntu 22.04, replace `noble` with `jammy`.*

* * * * *

 Step 2 -- Create a new VM to serve as your template
------------------------------------------------------

We'll give it basic resources (adjust if needed):

```
VMID=9000
VMNAME="ubuntu-24.04-cloudinit"
STORAGE="ceph-ssd"

qm create $VMID\
  --name $VMNAME\
  --memory 2048\
  --cores 2\
  --net0 virtio,bridge=vmbr0

```

* * * * *

 Step 3 -- Import the cloud image disk
---------------------------------------

```
qm importdisk $VMID noble-server-cloudimg-amd64.img $STORAGE --format qcow2

```

Attach it to the VM:

```
qm set $VMID --scsihw virtio-scsi-pci --scsi0 ${STORAGE}:vm-$VMID-disk-0

```

* * * * *

 Step 4 -- Add Cloud-Init drive
-------------------------------

Add a Cloud-Init device and mark it bootable:

```
qm set $VMID --ide2 ${STORAGE}:cloudinit
qm set $VMID --boot c --bootdisk scsi0

```

* * * * *

 Step 5 -- Configure serial console (for headless cloud images)
----------------------------------------------------------------

```
qm set $VMID --serial0 socket --vga serial0

```

* * * * *

 Step 6 -- Set default user & SSH key (optional now)
-----------------------------------------------------

If you want to test the template before Terraform:

```
qm set $VMID --ciuser ubuntu
qm set $VMID --sshkey ~/.ssh/id_rsa.pub

```

Terraform will override this later, but it's handy for manual testing.

* * * * *

 Step 7 -- Configure networking for DHCP (default)
---------------------------------------------------

If your lab uses DHCP on `vmbr0`, Cloud-Init will grab an IP automatically.\
If you prefer static IP assignment in Terraform, no change is needed --- you'll pass `ipconfig0` when cloning.

* * * * *

 Step 8 -- Convert the VM into a template
------------------------------------------

```
qm template $VMID

```

Now you have a golden image at `9000` that Terraform can clone and inject Cloud-Init data into automatically.

* * * * *

 Optional: verify
-------------------

To test:

```
qm clone 9000 9100 --name test-ubuntu --full true
qm set 9100 --ipconfig0 ip=dhcp
qm start 9100
qm terminal 9100

```

Log in as `ubuntu` (if you set the key or password) and confirm networking works.

* * * * *

 Summary
---------

**Template specs:**

| Setting | Value |
| --- | --- |
| Name | `ubuntu-24.04-cloudinit` |
| VMID | `9000` |
| Storage | `local-lvm` (adjust as needed) |
| Boot Disk | Imported `.img` |
| Boot Order | `scsi0` |
| Cloud-Init Drive | `ide2` |
| Network | `virtio`, `vmbr0` |
| Console | `serial0` |
| Status | Template |

