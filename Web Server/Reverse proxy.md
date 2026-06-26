**Definition:**  
A **reverse proxy is a server that sits between users and backend servers, receives user requests, and forwards those requests to the correct backend server. Then it sends the backend's response back to the user.**

### Easy Explanation

Think of a **reverse proxy as a receptionist in an office**.

- User → talks to receptionist
- Receptionist → sends request to the correct employee
- Employee → does the work
- Receptionist → gives the answer back to the user

The user does not directly communicate with the employee.

**Why Use Reverse Proxy?**
- **Load Balancing:** It distributes incoming traffic evenly across multiple backend servers so a single server does not get overloaded.

- **Enhanced Security:** It hides the identity and IP addresses of your backend servers, making it much harder for hackers to target them directly.

- **SSL Encryption/Decryption:** It can handle the heavy computing task of encrypting SSL/TLS traffic, freeing up resources for the backend servers. 

- **Caching:** It stores copies of popular web content locally to deliver pages to users faster without hitting the backend servers every time.- **Global Speed (CDN):** Services like Cloudflare use reverse proxies around the world to cache data closer to local users.