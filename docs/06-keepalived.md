# 06. Keepalived: Floating Virtual IP

## Idea

Two servers share one extra IP, the **VIP** (`172.16.3.100`). Only one owns it at a time. If the owner fails, the other takes it over automatically.

```
Normal:                          After Server 1 fails:

  Server 1 (150)  <-- VIP          Server 1 (down)
  Server 2 (100)                   Server 2 (100)  <-- VIP
```

## Terms

| Term | Meaning |
|---|---|
| VRRP | Protocol the servers use to elect the VIP owner |
| MASTER / BACKUP | Current owner / standby |
| Priority | Higher number wins (150 vs 100) |
| `virtual_router_id` | Same on both; identifies this VRRP group (51) |
| Advertisement | MASTER announces itself every `advert_int` second |
| Preempt | A returning higher-priority server takes the VIP back |
| Split-brain | Both think they are MASTER; usually a broken link between them |

If the BACKUP stops hearing advertisements for about 3 seconds, it promotes itself.

## Install (both servers)

```bash
sudo apt update
sudo apt install keepalived
sudo useradd -r -s /usr/sbin/nologin -M keepalived_script
```

The `keepalived_script` user runs the health-check script without root.

## Configure

Copy the matching example from `config/`:

```bash
# Server 1
sudo cp keepalived-server1.conf.example /etc/keepalived/keepalived.conf
# Server 2
sudo cp keepalived-server2.conf.example /etc/keepalived/keepalived.conf
```

Change `auth_pass` (max 8 characters, identical on both).

The config uses **unicast** between the two internal IPs, which is more predictable than multicast on VirtualBox. It also tracks `sshd`: if `sshd` stops, the server's priority drops by 60 and the peer takes over. Later you can swap this check for a database service.

## Start

```bash
sudo systemctl enable --now keepalived
systemctl status keepalived
```

## Verify

```bash
ip -br addr show enp0s8
```

Server 1 should list `172.16.3.100/24` alongside `172.16.3.11/24`. Server 2 should not.

From Server 2:

```bash
ping -c 3 172.16.3.100
ssh user@172.16.3.100    # lands on server1
```

## Failover test

On Server 1:

```bash
sudo systemctl stop keepalived
```

On Server 2, within a few seconds:

```bash
ip -br addr show enp0s8   # VIP now here
journalctl -u keepalived -e
```

Bring it back:

```bash
sudo systemctl start keepalived   # on Server 1; VIP returns to it
```

Optional: check the health script by stopping `ssh` on Server 1 (`sudo systemctl stop ssh`). Existing sessions stay open, but the VIP should move. Start it again afterwards.

## Notes

- The VIP exists only on the internal network. Windows reaches the servers through the forwarded ports (2031/2032), not the VIP.
- After a failover, SSH to the VIP shows a host-key warning because each server has its own key. This is expected.
- Never also configure the VIP in `/etc/network/interfaces`.

## Done when
- [ ] VIP is on Server 1 in normal operation
- [ ] Stopping keepalived on Server 1 moves the VIP to Server 2
- [ ] Starting it again returns the VIP to Server 1
- [ ] Screenshots saved in `screenshots/`
