# Simple High Availability Linux Infrastructure

Two Debian Minimal (CLI-only) servers in VirtualBox, connected over an internal network, reachable by SSH from the Windows host.

> **Status:** Midterm scope. Database replication and failover are planned for the final term (see bottom).

## Midterm Tasks

- [ ] Two Debian Minimal CLI VMs in VirtualBox (Server 2 is a Linked Clone of Server 1)
- [ ] Adapter 1 = NAT (Internet), Adapter 2 = Internal Network (server-to-server)
- [ ] Static IPs on the internal interface, assigned per group
- [ ] Server 1 and Server 2 can ping each other
- [ ] SSH installed and running on both
- [ ] Windows host can SSH into both via NAT port forwarding

## Architecture

```
                 Windows Host
                      |
                 VirtualBox
        ______________|______________
       |                             |
 +-----------+                 +-----------+
 | Server 1  |                 | Server 2  |
 |           |                 |           |
 | Adapter 1 |--NAT--> Internet <--NAT-- | Adapter 1 |
 | enp0s3    |                 | enp0s3    |
 |           |                 |           |
 | Adapter 2 |<-- Internal Network -->| Adapter 2 |
 | enp0s8    |      "intnet1"  | enp0s8    |
 | .11/24    |                 | .12/24    |
 +-----------+                 +-----------+

 Windows :2011 --(NAT port forward)--> Server 1 :22
 Windows :2012 --(NAT port forward)--> Server 2 :22
```

## Addressing Table

Assigned values.

| Item | Server 1 | Server 2 |
|---|---|---|
| Hostname | `server1` | `server2` |
| Adapter 1 (enp0s3) | NAT, DHCP | NAT, DHCP |
| Adapter 2 (enp0s8) | Internal Network `intnet1` | Internal Network `intnet1` |
| Internal static IP | `172.16.3.11/24` | `172.16.3.12/24` |
| SSH from Windows | `127.0.0.1:2031` | `127.0.0.1:2032` |

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
│   └── 06-troubleshooting.md
├── config/
│   ├── server1-interfaces.example
│   └── server2-interfaces.example
├── scripts/
│   └── check-network.sh            Midterm connectivity check
└── screenshots/                    Evidence for each step
```

## Quick Start

Follow the docs in order (01 → 05). Use `06-troubleshooting.md` when something fails.

## Final Term (Planned, Not Implemented Yet)

MySQL, replication, Keepalived/VRRP with a Virtual IP, Bash automation, and disaster recovery. Intentionally excluded from the midterm.
