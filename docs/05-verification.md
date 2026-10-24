# 05. Verification Checklist

Run through this after setup. Optional helper: `scripts/check-network.sh`.

## VirtualBox
- [ ] Two VMs exist: `server1`, `server2` (Server 2 is a Linked Clone)
- [ ] Both CLI-only (no desktop)
- [ ] Adapter 1 = NAT, Adapter 2 = Internal Network `intnet1` on both
- [ ] Port forwarding: 2031 to Server 1:22, 2032 to Server 2:22

## Inside each server
- [ ] `hostname` is correct and different on each
- [ ] `ip addr` shows static IP on `enp0s8`
- [ ] `ip route` shows a default route via NAT
- [ ] Static IP survives `sudo reboot`
- [ ] `ping -c 3 deb.debian.org` works (Internet via NAT)

## Server to server
- [ ] Server 1: `ping -c 3 172.16.3.12`
- [ ] Server 2: `ping -c 3 172.16.3.11`
- [ ] `ssh user@<other-server-ip>` works both ways

## From Windows
- [ ] `ssh user@127.0.0.1 -p 2031` reaches Server 1
- [ ] `ssh user@127.0.0.1 -p 2032` reaches Server 2

## Keepalived
- [ ] `keepalived` active on both servers
- [ ] VIP `172.16.3.10` is on Server 1's `enp0s8` (MASTER)
- [ ] From Server 2: `ping -c 3 172.16.3.10` works
- [ ] Stop keepalived on Server 1: VIP moves to Server 2 within a few seconds
- [ ] Start keepalived on Server 1: VIP moves back
- [ ] Everything survives a cold boot of both VMs

## Can you explain
- [ ] Why two adapters
- [ ] What `/24` means
- [ ] Why each server needs a different forwarded port
- [ ] How VRRP priority decides who owns the VIP
