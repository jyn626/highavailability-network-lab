# 02. VirtualBox Networking

## Why two adapters?

| Adapter | Mode | Purpose |
|---|---|---|
| 1 | NAT | Internet access (apt, updates). Host can reach in only via port forwarding. |
| 2 | Internal Network | Private link between Server 1 and Server 2. Not visible to the host or Internet. |

NAT alone cannot do server-to-server: each VM sits behind its own private NAT and cannot see the other.

## Mode comparison

| Mode | VM to Internet | VM to VM | Host to VM | Used here |
|---|---|---|---|---|
| NAT | Yes | No | Only via port forward | Adapter 1 |
| Internal Network | No | Yes (same name) | No | Adapter 2 |
| Host-Only | No | Yes | Yes | Not used |

## Configure (each VM must be powered off)
Settings > Network:
- **Adapter 1:** Enable, Attached to **NAT**
- **Adapter 2:** Enable, Attached to **Internal Network**, Name: `intnet1`

> The Internal Network name must be **identical** on both VMs (case-sensitive). A typo silently puts them on separate networks.

## Done when
- [ ] Both VMs show Adapter 1 = NAT, Adapter 2 = Internal Network (`intnet1`)
- [ ] `ip link` inside each VM lists `enp0s3` and `enp0s8`
- [ ] Screenshot saved in `screenshots/`
