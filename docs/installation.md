# Debian VM installation

## Prerequisites
- Oracle Virtualbox
- Debian netinst ISO Image

## Set up
| Settings | Server |
| -------- | -------- |
|  RAM  | 512 MB
| CPU | 2 cores
| Network Adapter 1 | NAT
| Network Adapter 2 | Internal Network


## Steps
1. Download Debian netinst ISO Image from https://www.debian.org/CD/netinst/
2. Create a new virtual machine in VirtualBox
3. Configure its RAM, CPU, and virtual disk.
4. Attach the Debian ISO to the virtual optical drive.
5. Start the VM and follow the Debian installer.
6. Create a user account (server) and set a hostname (server1).

## Linked Clone (final step)
after settting up server1, right click on it from the Virtualbox Manager `Machines` page and select `Clone`.

Select `Linked Clone`, and set the rest as default. Then update the `hostname` so it doesn't stay as `server1`

    sudo hostnamectl set-hostname server2