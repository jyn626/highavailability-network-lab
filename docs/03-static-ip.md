# 03. Static IP Configuration

## Reading `172.16.1.11/24`

| Part | Meaning |
|---|---|
| `172.16.3.11` | The host's address |
| `/24` | First 24 bits are the network = `255.255.255.0` |
| Network address | `172.16.3.0` |
| Host range | `172.16.3.1` to `172.16.3.254` (254 usable) |
| Broadcast | `172.16.3.255` |

Two hosts can talk directly only if they share the same network part. Your group's actual subnet may differ.

## Identify interfaces first

```bash
ip link
```

`enp0s3` should be Adapter 1 (NAT), `enp0s8` Adapter 2 (Internal). Confirm with the MAC addresses shown in VirtualBox.

## Configure
Debian Minimal uses `/etc/network/interfaces`. Reference files are in `config/`.

```bash
sudo nano /etc/network/interfaces
sudo systemctl restart networking
```

Rules:
- `enp0s3` stays on **DHCP** (NAT provides address, gateway, and DNS)
- `enp0s8` is **static**, with **no gateway** (internal network has no router; adding one conflicts with the NAT default route)

## Verify

```bash
ip addr show enp0s8      # shows the static IP
ip route                 # default via NAT; internal subnet via enp0s8
sudo reboot
ip addr show enp0s8      # still there after reboot = persistent
```

## Done when
- [ ] Static IP visible on `enp0s8` after a reboot
- [ ] Default route still points at the NAT interface
- [ ] Screenshot saved in `screenshots/`
