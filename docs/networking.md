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

## Configure Network Interface:
Edit the network interface file:

    sudo nano /etc/network/interfaces

Leave enp0s3 (Adapter 1) to DHCP:
    
    allow-hotplug enp0s3
    iface enp0s3 inet dhcp    

Set static IP for enp0s8 (Adapter 2):
    
    auto enp0s8
    iface enp0s8 inet static
        address 172.16.3.11
        netmask 255.255.255.0

Do the same for the other server, but assign a different static IP address.

## Inspect Network Configuration

Display network interfaces and their IP addresses to verify:

    ip addr

## Connectivity Tests

Test connectivity to an external IP address:

    ping -c 4 1.1.1.1

## Notes
