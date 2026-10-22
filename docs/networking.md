# Debian Networking Configuration

## Network Topology

Both Debian VMs (server1, server2) use VirtualBox NAT networking for outbound connectivity.

## Network Adapter Configuration

Adapter 1
| Setting | Configuration |
|---|---|
| State | Enabled |
| Attached to | NAT |
| IP assignment | DHCP (by default) |

Adapter 2
| Setting | Configuration |
|---|---|
| State | Enabled |
| Attached to | Internal Network |
| IP assignment | Static IP (e.g, 172.16.3.11) |

## Inspect Network Configuration

Display network interfaces and their IP addresses to verify:

    ip addr

## Connectivity Tests

Test connectivity to an external IP address:

    ping -c 4 1.1.1.1

## Notes
