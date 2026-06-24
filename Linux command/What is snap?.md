In Ubuntu, a **Snap** is a modern containerized software package designed by Canonical (the creators of Ubuntu). 
Unlike traditional installation methods, Snaps bundle an application and all its required dependencies (libraries, runtimes, config files) into a single, isolated file.

---

📦 Why Snaps Exist (The Core Benefits)

Traditionally, Linux software relied on system-wide shared libraries. If App A and App B needed different versions of the same library, it caused a conflict called "dependency hell." Snaps fix this.

- **Self-Contained:** Because everything is bundled inside, a Snap application works identical on Ubuntu, Fedora, Arch, or Mint without modification.
- **Strict Isolation (Sandbox):** Snaps run in a secure sandbox. They cannot access your private files, webcam, or hardware unless you explicitly grant permission
- **Automatic Updates:** Snaps update automatically in the background, keeping your software patched and secure.
- **Easy Rollbacks:** If an update breaks your software, a single command lets you roll back to the previous working version.
---

🔍 How Snaps Work Behind the Scenes

When you install a Snap, the system downloads a single compressed file (a `.snap` file using the SquashFS filesystem format).
1. **Mounting:** Ubuntu mounts this file directly onto your system inside the `/snap/` directory. It behaves like a read-only virtual drive.
2. **Execution:** When you run the app, it reads its program files straight from that mounted location, thinking it has its own private operating system to work with.

---

⚔️ Snap vs. APT (Traditional Packages)

You will see both types of software on your machine:

| Feature [[1](https://www.youtube.com/watch?v=9HuExVD56Bo), [2](https://training.linuxfoundation.org/resources/tutorials/get-started-with-snap-packages-in-linux/), [3](https://www.tecmint.com/install-snap-in-linux/), [4](https://phoenixnap.com/kb/snap-vs-apt), [5](https://lwn.net/Articles/825005/)] | Snap                                     | APT (`.deb`)                   |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- | ------------------------------ |
| **Package Size**                                                                                                                                                                                                                                                                                           | Larger (bundles everything) [1, 2]       | Smaller (shares system files)  |
| **Isolation**                                                                                                                                                                                                                                                                                              | Sandboxed/Isolated [1, 2]                | Full access to your system     |
| **Updates**                                                                                                                                                                                                                                                                                                | Automatic, managed by Snap daemon [1, 2] | Manual via `sudo apt upgrade`  |
| **Startup Speed**                                                                                                                                                                                                                                                                                          | Can be slower on the very first launch   | Instantly matches system speed |
Finding & Installing Apps

- **`snap find <app>`** — Searches the store for an app.
- **`sudo snap install <app>`** — Downloads and installs an app.
- **`sudo snap install <app> --classic`** — Instaps an app with normal system permissions (used for code editors like VS Code).

⚙️ Managing Installed Apps

- **`snap list`** — Shows all Snap apps currently on your computer.
- **`snap info <app>`** — Displays detailed information about an app (developer, version, size).
- **`snap connections <app>`**

🔄 Updating & Fixing Apps

- **`sudo snap refresh`** — Checks and updates all your Snap apps to the latest version.
- **`sudo snap refresh <app> --channel=<version>`** — Switches an app to a specific version or beta channel.
- **`sudo snap revert <app>`** — Instantly rolls an app back to its previous version if an update broke it. 

🗑️ Removing Apps

- **`sudo snap remove <app>`** — Completely deletes the app from your machine.
- **`snap saved`** — Shows backup snapshots of user data saved from previously removed apps. 