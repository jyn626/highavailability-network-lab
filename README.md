# Simple High Availability Linux Infrastructure

A hands-on learning project: two Debian Minimal (CLI-only) servers in VirtualBox, linked by a private network, reachable over SSH from Windows, with Keepalived providing a floating Virtual IP (VIP) for failover.

## What This Builds

- Two Debian Minimal CLI VMs in VirtualBox (Server 2 is a Linked Clone of Server 1)
- Adapter 1 = NAT (Internet), Adapter 2 = Internal Network (server-to-server)
- Static IPs on the internal interface
- SSH on both servers, reachable from Windows via NAT port forwarding
- Keepalived (VRRP) sharing one Virtual IP between the two servers

## Architecture

```
                 Windows Host
                      |
                 VirtualBox
        ______________|______________
       |                             |
 +-----------+                 +-----------+
 | Server 1  |                 | Server 2  |
 | MASTER    |                 | BACKUP    |
 |           |                 |           |
 | enp0s3 ---|--NAT--> Internet <--NAT---|--- enp0s3 |
 |           |                 |           |
 | enp0s8 ---|<-- Internal Network -->|--- enp0s8 |
 | .11/24    |      "intnet1"  | .12/24    |
 +-----------+                 +-----------+
        \                           /
         '--- VIP 172.16.3.10 ----'
          (lives on whichever server is MASTER)

 Windows :2031 --(NAT port forward)--> Server 1 :22
 Windows :2032 --(NAT port forward)--> Server 2 :22
```

## Addressing Table

| Item | Server 1 | Server 2 |
|---|---|---|
| Hostname | `server1` | `server2` |
| Adapter 1 (enp0s3) | NAT, DHCP | NAT, DHCP |
| Adapter 2 (enp0s8) | Internal Network `intnet1` | Internal Network `intnet1` |
| Internal static IP | `172.16.3.11/24` | `172.16.3.12/24` |
| SSH from Windows | `127.0.0.1:2031` | `127.0.0.1:2032` |
| Keepalived role | MASTER (priority 150) | BACKUP (priority 100) |
| Virtual IP | `172.16.3.10/24` (shared) | `172.16.3.10/24` (shared) |

## Repository Layout

```
.
├── README.md
├── .gitignore
├── server1.vbox / server2.vbox     VirtualBox VM definitions
├── docs/
│   ├── 01-vm-setup.md
│   ├── 02-virtualbox-networking.md
│   ├── 03-static-ip.md
│   ├── 04-ssh-and-port-forwarding.md
│   ├── 05-verification-checklist.md
│   ├── 06-troubleshooting.md
│   └── 07-keepalived.md
├── config/
│   ├── server1-interfaces.example
│   ├── server2-interfaces.example
│   ├── keepalived-server1.conf.example
│   └── keepalived-server2.conf.example
├── scripts/
│   └── check-network.sh            Connectivity + VIP check
└── screenshots/                    Evidence for each step
```

## Quick Start

Follow the docs in order (01 to 07). Use `06-troubleshooting.md` when something fails.

## Roadmap

- [x] VirtualBox VMs, dual adapters, static IPs
- [x] SSH and NAT port forwarding
- [x] Keepalived with a floating VIP
- [ ] MySQL on both servers
- [ ] Master-master replication
- [ ] Bash automation and failure recovery
