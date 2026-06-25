In Ubuntu, a **service** (often called a **daemon**) is ==a program that runs silently in the background without a user interface==. Unlike a regular app (like a web browser) that closes when you exit, services start automatically when Ubuntu boots up and remain active until the system shuts down

Common Examples of Services

- **Networking (`NetworkManager`)**: Connects your computer to the internet.
- **Containers (`docker`)**: The background engine managing your containers, as seen in your active system logs.
- **Remote Access (`ssh`)**: Listens continuously for encrypted remote connections.
- **Web Servers (`nginx` or `apache2`)**: Constantly waits to deliver web pages to users.

How Services are Managed: `systemd` and `systemctl`

Modern Ubuntu releases use a service framework manager called **`systemd`**. It acts like an orchestra conductor, launching background processes simultaneously to dramatically accelerate your system's boot speeds. 

You control `systemd` using the **`systemctl`** tool in your terminal.

| Task                  | Command Syntax                     | Description                                                 |
| --------------------- | ---------------------------------- | ----------------------------------------------------------- |
| **Check Health**      | `systemctl status <service>`       | Shows if a service is actively running or crashed.          |
| **Start**             | `sudo systemctl start <service>`   | Launches a stopped service immediately.                     |
| **Stop**              | `sudo systemctl stop <service>`    | Safely halts a running service.                             |
| **Restart**           | `sudo systemctl restart <service>` | Power-cycles a service to apply configuration edits.        |
| **Enable Auto-Boot**  | `sudo systemctl enable <service>`  | Configures a service to start on every system startup.      |
| **Disable Auto-Boot** | `sudo systemctl disable <service>` | Prevents a service from starting up when the machine boots. |

