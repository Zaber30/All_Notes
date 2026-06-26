Phase 1: Finding the Server (The DNS Lookup)

Before your browser can talk to a server, it needs to find its address. 

- **1. Browser Cache Check:** The browser checks its internal history to see if it already knows the IP address for that domain.
- **2. OS & Router Cache Check:** If not found, it checks the computer's Operating System cache and local Wi-Fi router cache.
- **3. ISP DNS Query:** The request travels to your Internet Service Provider (ISP) Recursive DNS resolver.
- **4. Root & TLD Nameservers:** The resolver asks Root servers, then Top-Level Domain (TLD) servers (like `.com` or `.org`) who owns the domain.
- **5. Authoritative DNS Server:** The resolver gets the final, exact IP address (e.g., `192.0.2.1`) from the domain's registrar and hands it back to the browser. 
Phase 2: Establishing a Connection (The Handshake)

Now that the browser knows _where_ the server is, it opens a secure communication channel.

- **6. TCP Handshake:** The browser sends a `SYN` packet to the server, the server replies with a `SYN-ACK`, and the browser responds with an `ACK`. This creates a stable connection. 
- **7. TLS/SSL Handshake:** For secure sites (`https://`), the browser and server exchange security certificates, agree on an encryption method, and generate secret keys to encrypt all future data. 
Phase 3: Processing the Request on the Server

The request now arrives at the actual server infrastructure.

- **8. Firewall Security Check:** The hardware or software firewall scans the incoming request to ensure it is safe and not part of a cyber attack.
- **9. Nginx (Web Server) Reception:** The request hits Nginx (the outer gatekeeper) listening on Port 80 (HTTP) or Port 443 (HTTPS).
- **10. Static vs. Dynamic Evaluation:** Nginx reads its configuration files to see what the user wants:
    - **If Static (Images/HTML):** Nginx reads the file directly from the hard drive and skips straight to Step 14.
    - **If Dynamic (User Data/Logins):** Nginx acts as a Reverse Proxy and forwards the request to the Application Server.
- **11. Application Server Execution:** The application server (like Node.js, Python Gunicorn, or PHP-FPM) executes the actual backend code logic.
- **12. Database Query:** If the code needs stored information, the application server requests data from the Database Server (like MySQL or PostgreSQL).
- **13. Response Construction:** The application server puts the data together into a web readable format (usually HTML or JSON) and passes it back to Nginx.

Phase 4: Delivering the Content

The data travels back to the user's screen. 

- **14. Nginx Response Assembly:** Nginx adds security headers, compresses the data (using Gzip or Brotli) to make it smaller, and sends it back across the internet.
- **15. Browser Rendering:** The browser receives the raw code, parses the HTML structure, downloads any missing CSS/images, and displays the final visual webpage on the screen. 