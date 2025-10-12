# Ubuntu Notes
adding more space and extending LVM

```bash
# allocate more space on hypervisor
# make the partition see the space (dev/sda3, Resize, Write, quit)
sudo cfdisk
# extend the physical volume from the partition
sudo pvresize /dev/sda3
# extend LV to use up all space from VG
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
# resize file system
sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
# check can see the space on filesystem
df -h
```


## Remove ssh banner
Remove folder `/etc/update-motd.d/`

## Set Static IP
Edit file `/etc/netplan/50-cloud-init.yaml`

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens18:
      dhcp4: no
      addresses:
        - 192.168.1.27/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [192.168.1.1]
```

`sudo netplan apply`

## Change Hostname
Change it in /etc/hosts and /etc/hostname

## Install Docker

## Add User
`sudo adduser username`
`usermod -aG sudo username`

## Fix date and time

`sudo timedatectl set-timezone America/Boise`

## Disable IPV6
I successfully disabled IPv6 once putting the following lines in `/etc/sysctl.conf`:

```
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

also run this command to load changes

```
sudo sysctl -p
```


## Clean up disk space
- `sudo journalctl --vacuum-size=300M` reduces the logs to 300 MB
- `sudo logrotate /etc/logrotate.conf` compresses or (?) deletes system logs
- `sudo apt-get autoremove` removes software that are only dependencies to packages you removed earlier and don't need anymore
- `sudo apt-get clean` this is nearly the same as `sudo rm -rf /var/cache/apt/archives/*` and deletes cached downloaded packages for installation. Running this during some installations could be a problem.