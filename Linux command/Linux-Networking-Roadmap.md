# 🌐 Linux Networking: Beginner to Advanced

A dedicated deep-dive into Linux networking. Pairs with the main **Linux Learning Roadmap** — link this note under `03-Advanced/` or as its own MOC.

---

## 🟢 Level 1: Beginner (Core Concepts)

- **Networking Fundamentals** — IP addresses, subnets, MAC addresses, ports, protocols
- **OSI vs TCP/IP Model** — Layers explained, where Linux tools operate in each layer
- **IPv4 vs IPv6 Basics** — Addressing formats, private vs public IPs
- **Network Interfaces** — `eth0`, `wlan0`, `lo`, viewing interfaces with `ip link` / `ifconfig`
- **Checking IP Configuration** — `ip addr`, `ifconfig`, `hostname -I`
- **Basic Connectivity Testing** — `ping`, `traceroute`/`tracepath`, `mtr`
- **DNS Basics** — What DNS does, `/etc/resolv.conf`, `/etc/hosts`, `nslookup`, `dig`, `host`
- **Basic Troubleshooting Tools** — `ping`, `ip`, `curl`, `wget`
- **Common Ports & Protocols** — HTTP(80), HTTPS(443), SSH(22), FTP(21), DNS(53), etc.
- **Network Configuration Files (Overview)** — Where configs live per distro (Netplan, NetworkManager, `/etc/network/interfaces`)

---

## 🟡 Level 2: Intermediate (Configuration & Services)

- **Static vs Dynamic IP Configuration** — Setting static IPs, DHCP basics
- **NetworkManager** — `nmcli`, `nmtui`, managing connections
- **Netplan (Ubuntu/Debian-based)** — YAML configs, applying changes
- **systemd-networkd** — Alternative network management on systemd distros
- **Routing Basics** — Routing tables, `ip route`, default gateway, static routes
- **DNS Resolution Deep Dive** — `systemd-resolved`, DNS caching, custom DNS servers
- **Firewalls (Basic)** — `ufw` (Uncomplicated Firewall), enabling/disabling rules, allow/deny ports
- **SSH Networking** — Port forwarding, tunneling, `ssh -L`/`-R`/`-D`
- **File Transfer Over Network** — `scp`, `sftp`, `rsync` over SSH
- **Network Diagnostics Tools** — `ss` (socket stats), `netstat` (legacy), `nmap` basics
- **Wireless Networking** — `iwconfig`, `iw`, connecting via CLI (`nmcli`, `wpa_supplicant`)
- **Hosts File & Local Resolution** — Overriding DNS locally for testing
- **Basic Network Services** — Setting up a simple web server, FTP server, or SSH server

---

## 🟠 Level 3: Advanced (System & Network Administration)

- **Advanced Firewalling** — `iptables` (chains, tables, NAT), `nftables` (modern replacement)
- **Network Address Translation (NAT)** — Masquerading, port forwarding, SNAT/DNAT
- **VPNs on Linux** — OpenVPN, WireGuard setup and configuration
- **Bridging & Bonding** — Creating network bridges, NIC bonding/teaming for redundancy
- **VLANs** — Tagging, trunking, creating VLAN interfaces in Linux
- **Advanced Routing** — Policy-based routing, multiple routing tables, `ip rule`
- **Network Namespaces** — Isolated network stacks, use in containers
- **Traffic Control (`tc`)** — Bandwidth shaping, QoS, simulating latency/packet loss
- **Packet Analysis** — `tcpdump`, Wireshark (CLI/GUI), reading packet captures
- **DNS Server Setup** — BIND9, dnsmasq, running your own DNS resolver/authoritative server
- **DHCP Server Setup** — `isc-dhcp-server`, `dnsmasq` DHCP
- **Load Balancing** — HAProxy, Nginx as a reverse proxy/load balancer
- **Network Security** — Fail2ban, port scanning detection, hardening SSH/firewall rules
- **Proxy Servers** — Squid, forward vs reverse proxies

---

## 🔴 Level 4: Expert (Specialized & Enterprise Networking)

- **Deep Packet Inspection & Analysis** — Advanced Wireshark filters, protocol dissection
- **Software-Defined Networking (SDN)** — Open vSwitch, SDN concepts on Linux
- **Container Networking** — Docker networking modes (bridge, host, overlay), CNI plugins, Kubernetes networking
- **eBPF & XDP** — Kernel-level packet processing, high-performance networking, tools like `bpftrace`
- **High Availability Networking** — Keepalived, VRRP, failover clusters
- **BGP & Dynamic Routing Protocols** — FRRouting, Quagga, BGP/OSPF on Linux routers
- **Network Performance Tuning** — Kernel network parameters (`sysctl`), TCP tuning, buffer sizes
- **IPSec & Advanced VPN Architectures** — Site-to-site VPNs, StrongSwan, Libreswan
- **Network Monitoring at Scale** — Prometheus + Grafana for network metrics, SNMP, Zabbix/Nagios
- **Building a Linux Router/Firewall** — Turning Linux into a full router (pfSense-like setup manually)
- **Multicast Networking** — IGMP, multicast routing on Linux
- **Cloud Networking Integration** — VPCs, security groups, Linux networking in AWS/GCP/Azure
- **Zero Trust & Advanced Security Architectures** — Network segmentation, microsegmentation with Linux tools
- **Custom Kernel Network Stack Tuning** — Compiling kernel with custom network drivers/modules

---

## 🛠️ Essential Command Reference (Quick List)

| Category | Commands |
|---|---|
| Interface Info | `ip addr`, `ip link`, `ifconfig` |
| Routing | `ip route`, `route`, `ip rule` |
| Connectivity | `ping`, `traceroute`, `mtr`, `curl`, `wget` |
| DNS | `dig`, `nslookup`, `host`, `resolvectl` |
| Sockets/Ports | `ss`, `netstat`, `lsof -i` |
| Firewall | `iptables`, `nftables`, `ufw`, `firewalld` |
| Packet Capture | `tcpdump`, `wireshark`, `tshark` |
| Wireless | `iwconfig`, `iw`, `nmcli` |
| Scanning | `nmap` |
| Bandwidth | `iftop`, `nload`, `vnstat` |

---

## 📚 Suggested Obsidian Structure

```
Linux Networking MOC
├── 01-Beginner/
├── 02-Intermediate/
├── 03-Advanced/
├── 04-Expert/
└── Cheatsheets/
    ├── iptables-cheatsheet.md
    ├── tcpdump-cheatsheet.md
    └── ssh-tunneling-cheatsheet.md
```

> 💡 Tip: Tag notes with `#linux/networking/beginner`, `#linux/networking/advanced`, etc. Link this file back to your main **Linux Learning Roadmap** as a sub-MOC.
