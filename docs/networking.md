# Debian Networking Configuration

## Prerequisites

*server1*
| Setting | IP address/Port |
|---|---|
| Internal Network IP | 172.16.3.11 |
| Host Port | 2031 |

*server2*
| Setting | IP address/Port |
|---|---|
| Internal Network IP | 172.16.3.12 |
| Host Port | 2032 |

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

Do the same for the other server, but assign a different static IP address (e.g, 172.16.3.12).

Restart the system's network to apply the changes

    sudo systemctl restart networking

## Inspect Network Configuration

Display network interfaces and their IP addresses to verify:

    ip addr

## Connectivity Tests

Test connectivity to verify both servers :

*from server1, ping server2*

    ping -c 172.16.3.12

*from server2, ping server1*

    ping -c 172.16.3.11

## Set up Port Forwading
Set a Port Forwading rule on each VMs, this rule is important for our Host OS to have access on our server's SSH.

Set Rule:
-   Host Port: *(view prerequisites)*
-   Guest Port: 22
-   *leave other options as empty, it'll default to localhost...*

# Test SSH


## Notes
