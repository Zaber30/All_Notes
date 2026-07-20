Logical network ports (0 to 65535) are officially categorized by the Internet Assigned Numbers Authority (IANA) into three distinct ranges based on their use and assignment rules. 

IANA Port Ranges

- **Well-Known Ports (0 – 1023)**: Reserved for core internet services, protocols, and system processes. Operating systems require administrative privileges to bind to these ports.
- **Registered Ports (1024 – 49151)**: Assigned by IANA to specific companies or developers for proprietary applications and software services (e.g., databases, games).
- **Dynamic / Private Ports (49152 – 65535)**: Never permanently assigned to any application. Used as temporary "ephemeral" ports for client-side outbound communications
Popular TCP Ports

TCP ports are connection-oriented. They require a handshake and guarantee that all data arrives safely and in the correct order. 

- **Port 21:** FTP (File Transfer Protocol) for unencrypted file transfers
- **Port 22:** SSH / SFTP for secure remote server management and secure file transfers
- **Port 23:** Telnet for legacy, unencrypted text-based remote access
- **Port 25:** SMTP (Simple Mail Transfer Protocol) for routing email between servers
- **Port 80:** HTTP (Hypertext Transfer Protocol) for unencrypted web browsing
- **Port 110:** POP3 for downloading email from a server unencrypted
- **Port 143:** IMAP for synchronizing email across devices unencrypted
- **Port 443:** HTTPS (HTTP Secure) for encrypted, secure web browsing
- **Port 445:** SMB (Server Message Block) for sharing files and printers on Windows networks
- **Port 465:** SMTPS for sending outbound email securely over SSL/TLS
- **Port 587:** SMTP Submission, the modern standard port for client email sending
- **Port 993:** IMAPS for secure, encrypted email synchronization
- **Port 995:** POP3S for secure, encrypted email downloading
- **Port 1433:** MSSQL default port for Microsoft SQL Server databases
- **Port 2049:** NFS (Network File System) for sharing files on Linux networks
- **Port 3306:** MySQL default database connection port
- **Port 3389:** RDP (Remote Desktop Protocol) for graphical remote access to Windows
- **Port 5432:** PostgreSQL default database connection port
- **Port 6379:** Redis default in-memory database and caching port
- **Port 27017:** MongoDB default NoSQL database port
---

Popular UDP Ports

UDP ports are connectionless. They do not verify if the receiving device gets the data, making them much faster and ideal for real-time streaming, gaming, and quick lookups. 

- **Port 53:** DNS (Domain Name System) for translating website domain names to IP addresses (can also use TCP for large transfers)
- **Port 67, 68:** DHCP (Dynamic Host Configuration Protocol) for automatically assigning IP addresses to devices on a network
- **Port 69:** TFTP (Trivial File Transfer Protocol) for fast, simple file transfers without authentication
- **Port 88:** Kerberos for handling secure network authentication tokens
- **Port 123:** NTP (Network Time Protocol) for synchronizing system clocks across the internet
- **Port 161, 162:** SNMP (Simple Network Management Protocol) for monitoring and managing network hardware
- **Port 389:** LDAP (Lightweight Directory Access Protocol) for user directory lookups
- **Port 500:** ISAKMP/IKE for setting up secure VPN tunnels for IPSec traffic
- **Port 5060, 5061:** SIP (Session Initiation Protocol) for establishing VoIP voice and video calls
- **Port 1935:** RTMP (Real-Time Messaging Protocol) for streaming live video and audio