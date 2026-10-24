# 04. SSH and Port Forwarding

## Terms

| Term | Meaning |
|---|---|
| SSH client | Program that connects out (`ssh`) |
| SSH server (`sshd`) | Service that accepts connections on port 22 |
| Host key | Server's identity; you are asked to trust it on first connect |

## Ensure sshd is running (both servers)

```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl enable --now ssh
systemctl status ssh
ss -tlnp | grep :22
```

## Three connection paths

| Path | Command | Goes over |
|---|---|---|
| Server 1 to Server 2 | `ssh user@172.16.3.12` | Internal Network |
| Windows to Server 1 | `ssh user@127.0.0.1 -p 2031` | NAT port forward |
| Windows to Server 2 | `ssh user@127.0.0.1 -p 2032` | NAT port forward |

## Port forwarding rules
VM Settings > Network > Adapter 1 > Advanced > Port Forwarding.

| VM | Name | Protocol | Host Port | Guest Port |
|---|---|---|---|---|
| server1 | ssh | TCP | 2031 | 22 |
| server2 | ssh | TCP | 2032 | 22 |

Host IP and Guest IP can stay blank.

Why different host ports: both VMs listen on 22 inside, but Windows has only one port 2031 and one 2032. Each host port maps to exactly one VM.

Optional CLI equivalent (VM powered off):

```bat
VBoxManage modifyvm "server1" --natpf1 "ssh,tcp,,2031,,22"
VBoxManage modifyvm "server2" --natpf1 "ssh,tcp,,2032,,22"
```

## Windows host-key warnings
If you rebuild a VM, Windows may report a changed host key. Remove the stale entry:

```bat
ssh-keygen -R "[127.0.0.1]:2031"
```

## Done when
- [ ] `sshd` active on both servers
- [ ] Server 1 to Server 2 SSH works
- [ ] Windows to Server 1 and Server 2 SSH both work
- [ ] Screenshots saved in `screenshots/`
