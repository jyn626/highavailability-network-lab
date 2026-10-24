# 01. VM Setup

## Goal
Two Debian Minimal CLI-only VMs. Build Server 1 once, then clone it.

## Server 1 (base VM)

| Setting | Value |
|---|---|
| Name | `server1` |
| Type / Version | Linux / Debian (64-bit) |
| RAM | 512 MB works |
| CPU | 1 |
| Disk | 10 GB, VDI, dynamically allocated |
| ISO | Debian netinst |

## Install choices
- Non-graphical install
- Hostname: `server1`
- Software selection: **uncheck** desktop environment and GNOME; **keep** *SSH server* and *standard system utilities*
- After install: remove the ISO from Settings > Storage

## Take a snapshot
Name it `clean-install` so you can roll back.

## Create Server 2 (Linked Clone)
1. Right-click `server1` > **Clone**
2. Name: `server2`
3. Check **Reinitialize the MAC address of all network cards**
4. Clone type: **Linked clone** (shares the parent's disk, saves space)
5. Clone from the `clean-install` snapshot

> A Linked Clone depends on the parent's disk. Do not delete `server1` or its snapshot.

## Fix clone duplicates (run on Server 2 only)

```bash
sudo hostnamectl set-hostname server2
sudo nano /etc/hosts                 # change server1 -> server2
sudo rm /etc/machine-id /var/lib/dbus/machine-id
sudo systemd-machine-id-setup
sudo rm /etc/ssh/ssh_host_*
sudo dpkg-reconfigure openssh-server
sudo reboot
```

Why: a clone copies the hostname, machine ID, and SSH host keys. Duplicates cause confusing identity and SSH warnings.

## Done when
- [ ] Both VMs boot to a text login
- [ ] `hostname` prints the right name on each
- [ ] Screenshot saved in `screenshots/`
