# Linux Networking Commands (With Description)

---

# 1. Network Interfaces

## Show all interfaces

```
ip link show
```

👉 Shows all network interfaces (eth0, wlan0, lo)

---

## Show IP addresses

```
ip addr show
```

👉 Displays IP address, subnet, MAC address

---

## Bring interface up

```
sudo ip link set eth0 up
```

👉 Activates network interface

---

## Bring interface down

```
sudo ip link set eth0 down
```

👉 Disables network interface

---

# 2. IP Address Management

## Add IP address

```
sudo ip addr add 192.168.1.10/24 dev eth0
```

👉 Assign IP manually (temporary)

---

## Remove IP address

```
sudo ip addr del 192.168.1.10/24 dev eth0
```

👉 Remove assigned IP

---

# 3. Routing

## Show routing table

```
ip route show
```

👉 Shows how packets are routed

---

## Add route

```
sudo ip route add 10.0.0.0/24 via 192.168.1.1
```

👉 Send traffic for 10.0.0.0 network via gateway

---

## Default gateway

```
ip route | grep default
```

👉 Shows internet gateway

---

# 4. DNS & Hostname

## Show hostname

```
hostname
```

👉 Displays system name

---

## Test DNS resolution

```
nslookup google.com
```

👉 Converts domain → IP

---

## DNS lookup (modern)

```
dig google.com
```

👉 Detailed DNS info

---

# 5. Connectivity Testing

## Ping host

```
ping google.com
```

👉 Check if network is reachable

---

## Trace route path

```
traceroute google.com
```

👉 Shows packet path to destination

---

# 6. Socket & Connection Monitoring

## Show active connections

```
ss -tulpn
```

👉 Shows open ports and services

---

## Show listening ports only

```
ss -ltn
```

👉 Displays only listening TCP ports

---

# 7. Packet Capture

## Capture packets

```
sudo tcpdump -i eth0
```

👉 Shows live network traffic

---

## Capture HTTP traffic

```
sudo tcpdump port 80
```

👉 Filters port 80 traffic

---

## Save packets

```
sudo tcpdump -w file.pcap
```

👉 Save traffic for Wireshark

---

# 8. ARP / MAC Address

## Show ARP table

```
ip neigh
```

👉 IP ↔ MAC mapping

---

## Clear ARP cache

```
sudo ip neigh flush all
```

👉 Removes stored neighbor entries

---

# 9. Firewall (nftables / iptables)

## Show iptables rules

```
sudo iptables -L
```

👉 Lists firewall rules

---

## Block port 22 (SSH)

```
sudo iptables -A INPUT -p tcp --dport 22 -j DROP
```

👉 Blocks SSH access

---

## Show nftables rules

```
sudo nft list ruleset
```

👉 Modern firewall system

---

# 10. Network Performance

## Interface stats

```
ip -s link
```

👉 Shows packet errors, drops, traffic

---

## Bandwidth test

```
iperf3 -siperf3 -c server_ip
```

👉 Measures network speed

---

## Show kernel network settings

```
sysctl -a | grep net
```

👉 Displays Linux networking parameters

---

# 11. Traffic Control (QoS)

## Limit bandwidth

```
tc qdisc add dev eth0 root tbf rate 1mbit
```

👉 Limits speed to 1 Mbps

---

## Remove limit

```
tc qdisc del dev eth0 root
```

👉 Removes traffic control

---

# 12. Network Debugging

## Show open ports + processes

```
ss -tulpn
```

---

## Check network interface errors

```
ip -s link
```

---

## Continuous ping

```
ping -c 5 google.com
```

👉 Sends 5 packets only

---

# 13. Wireless (WiFi)

## Show WiFi networks

```
nmcli dev wifi list
```

---

## Connect WiFi

```
nmcli dev wifi connect "SSID" password "password"
```

---

# 14. Basic Flow of Networking in Linux

```
Application   ↓Socket   ↓TCP / UDP   ↓IP Layer   ↓Network Interface (eth0)   ↓Hardware
```