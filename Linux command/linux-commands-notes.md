# Linux Commands — Basic to Advanced

A complete reference of Linux commands with descriptions, grouped by category and skill level.

---

## 1. Basic Navigation & File Management

| Command             | Description                                            |
| ------------------- | ------------------------------------------------------ |
| `pwd`               | Print current working directory                        |
| `ls`                | List files and directories                             |
| `ls -l`             | Long listing format (permissions, owner, size, date)   |
| `ls -a`             | Show hidden files (starting with `.`)                  |
| `ls -lh`            | Long listing with human-readable file sizes            |
| `cd <dir>`          | Change directory                                       |
| `cd ..`             | Move up one directory                                  |
| `cd ~`              | Go to home directory                                   |
| `cd -`              | Go to previous directory                               |
| `mkdir <dir>`       | Create a new directory                                 |
| `mkdir -p a/b/c`    | Create nested directories                              |
| `rmdir <dir>`       | Remove empty directory                                 |
| `rm <file>`         | Remove a file                                          |
| `rm -r <dir>`       | Remove directory recursively                           |
| `rm -rf <dir>`      | Force remove directory without prompts (use carefully) |
| `cp <src> <dst>`    | Copy file                                              |
| `cp -r <src> <dst>` | Copy directory recursively                             |
| `mv <src> <dst>`    | Move or rename file/directory                          |
| `touch <file>`      | Create empty file or update timestamp                  |
| `cat <file>`        | Print file contents                                    |
| `tac <file>`        | Print file contents in reverse line order              |
| `less <file>`       | View file page-by-page (scrollable)                    |
| `more <file>`       | View file page-by-page (simpler than less)             |
| `head <file>`       | Show first 10 lines of file                            |
| `head -n 20 <file>` | Show first 20 lines                                    |
| `tail <file>`       | Show last 10 lines of file                             |
| `tail -f <file>`    | Follow file in real time (e.g., logs)                  |
| `file <file>`       | Show file type                                         |
| `stat <file>`       | Show detailed file metadata                            |
| `tree`              | Show directory structure as a tree (may need install)  |

---

## 2. File Permissions & Ownership

| Command | Description |
|---|---|
| `chmod 755 <file>` | Change file permissions (numeric) |
| `chmod +x <file>` | Make file executable |
| `chmod -R 755 <dir>` | Recursively change permissions |
| `chown user:group <file>` | Change file owner and group |
| `chown -R user:group <dir>` | Recursively change ownership |
| `umask` | Show/set default permission mask |
| `ls -l` | View permissions (`rwxr-xr-x` format) |
| `getfacl <file>` | Show Access Control List (ACL) |
| `setfacl -m u:user:rwx <file>` | Set ACL for a specific user |

**Permission basics:** `r` = read (4), `w` = write (2), `x` = execute (1). Format: `chmod [owner][group][others]`.

---

## 3. Searching & Finding

| Command | Description |
|---|---|
| `find <path> -name "*.txt"` | Find files by name pattern |
| `find <path> -type f` | Find only regular files |
| `find <path> -type d` | Find only directories |
| `find <path> -mtime -7` | Find files modified in last 7 days |
| `find <path> -size +100M` | Find files larger than 100MB |
| `find <path> -exec cmd {} \;` | Execute a command on each found file |
| `locate <file>` | Quickly find file using prebuilt index |
| `which <command>` | Show path of a command's executable |
| `whereis <command>` | Show binary, source, and man page locations |
| `grep "pattern" <file>` | Search text matching pattern in file |
| `grep -r "pattern" <dir>` | Recursively search in directory |
| `grep -i "pattern"` | Case-insensitive search |
| `grep -v "pattern"` | Invert match (show non-matching lines) |
| `grep -n "pattern"` | Show line numbers of matches |
| `egrep` / `grep -E` | Extended regex grep |

---

## 4. Text Processing

| Command | Description |
|---|---|
| `awk '{print $1}' file` | Pattern scanning & text processing (column extraction) |
| `sed 's/old/new/g' file` | Stream editor — find and replace text |
| `sed -i 's/old/new/g' file` | In-place find and replace |
| `cut -d',' -f1 file` | Extract fields/columns by delimiter |
| `sort file` | Sort lines alphabetically |
| `sort -n file` | Sort numerically |
| `sort -r file` | Sort in reverse order |
| `uniq file` | Remove duplicate adjacent lines |
| `uniq -c file` | Count occurrences of duplicate lines |
| `wc file` | Count lines, words, characters |
| `wc -l file` | Count lines only |
| `tr 'a-z' 'A-Z'` | Translate/transform characters |
| `diff file1 file2` | Show differences between two files |
| `cmp file1 file2` | Compare two files byte by byte |
| `paste file1 file2` | Merge lines of files side by side |
| `xargs` | Build and execute commands from input |
| `column -t file` | Format input into aligned columns |

---

## 5. Process Management

| Command | Description |
|---|---|
| `ps` | Show running processes for current shell |
| `ps aux` | Show all running processes (detailed) |
| `ps -ef` | Show all processes (full format) |
| `top` | Real-time process monitor |
| `htop` | Enhanced interactive process viewer (may need install) |
| `kill <pid>` | Terminate process by PID |
| `kill -9 <pid>` | Force kill a process |
| `killall <name>` | Kill all processes by name |
| `pkill <name>` | Kill process(es) matching name |
| `bg` | Resume a job in background |
| `fg` | Bring background job to foreground |
| `jobs` | List background jobs |
| `nohup <cmd> &` | Run command immune to hangups, in background |
| `nice -n 10 <cmd>` | Run command with adjusted priority |
| `renice 10 -p <pid>` | Change priority of running process |
| `pgrep <name>` | Find PID by process name |
| `pstree` | Show process tree |
| `disown` | Remove job from shell's job table |
| `&` (suffix) | Run command in background |
| `Ctrl+Z` | Suspend current foreground process |
| `Ctrl+C` | Terminate current foreground process |

---

## 6. System Information & Monitoring

| Command               | Description                                           |
| --------------------- | ----------------------------------------------------- |
| `uname -a`            | Show all system/kernel information                    |
| `hostname`            | Show system hostname                                  |
| `uptime`              | Show how long system has been running                 |
| `whoami`              | Show current logged-in user                           |
| `who`                 | Show who is logged in                                 |
| `w`                   | Show logged-in users and their activity               |
| `id`                  | Show user and group IDs                               |
| `df -h`               | Show disk space usage (human-readable)                |
| `du -sh <dir>`        | Show directory size summary                           |
| `du -h --max-depth=1` | Show size of subdirectories one level deep            |
| `free -h`             | Show memory usage (human-readable)                    |
| `vmstat`              | Show virtual memory statistics                        |
| `iostat`              | Show CPU and I/O statistics                           |
| `lscpu`               | Show CPU architecture details                         |
| `lsblk`               | List block devices (disks/partitions)                 |
| `lsusb`               | List USB devices                                      |
| `lspci`               | List PCI devices                                      |
| `dmesg`               | Show kernel ring buffer messages (boot/hardware logs) |
| `date`                | Show or set system date/time                          |
| `cal`                 | Show calendar                                         |
| `uptime -p`           | Show uptime in pretty format                          |
| `last`                | Show login history                                    |
| `history`             | Show command history                                  |

---

## 7. Networking

| Command                    | Description                                         |
| -------------------------- | --------------------------------------------------- |
| `ip a`                     | Show IP addresses of network interfaces             |
| `ip route`                 | Show routing table                                  |
| `ifconfig`                 | Show/configure network interfaces (legacy)          |
| `ping <host>`              | Test connectivity to a host                         |
| `curl <url>`               | Transfer data from/to a URL                         |
| `curl -O <url>`            | Download a file                                     |
| `wget <url>`               | Download files from the web                         |
| `netstat -tulnp`           | Show listening ports and services (legacy)          |
| `ss -tulnp`                | Show socket statistics (modern netstat replacement) |
| `traceroute <host>`        | Trace route packets take to a host                  |
| `nslookup <domain>`        | Query DNS information                               |
| `dig <domain>`             | Detailed DNS lookup tool                            |
| `host <domain>`            | Simple DNS lookup                                   |
| `scp file user@host:/path` | Securely copy files over network                    |
| `rsync -avz src dst`       | Efficiently sync files/directories                  |
| `ssh user@host`            | Connect to remote host via SSH                      |
| `ssh -i key.pem user@host` | SSH using a specific private key                    |
| `nc -zv host port`         | Test if a port is open (netcat)                     |
| `iptables -L`              | List firewall rules (legacy)                        |
| `ufw status`               | Show firewall status (Uncomplicated Firewall)       |
| `nmcli`                    | Manage NetworkManager connections                   |

---

## 8. Package Management

### Debian/Ubuntu (APT)
| Command | Description |
|---|---|
| `apt update` | Refresh package index |
| `apt upgrade` | Upgrade installed packages |
| `apt install <pkg>` | Install a package |
| `apt remove <pkg>` | Remove a package |
| `apt purge <pkg>` | Remove package + config files |
| `apt search <pkg>` | Search for a package |
| `dpkg -i <file.deb>` | Install a local .deb package |

### RHEL/CentOS/Fedora (YUM/DNF)
| Command | Description |
|---|---|
| `yum install <pkg>` | Install package (older RHEL) |
| `dnf install <pkg>` | Install package (modern Fedora/RHEL) |
| `rpm -ivh <file.rpm>` | Install local .rpm package |

---

## 9. Archiving & Compression

| Command | Description |
|---|---|
| `tar -cvf archive.tar dir/` | Create a tar archive |
| `tar -xvf archive.tar` | Extract a tar archive |
| `tar -czvf archive.tar.gz dir/` | Create gzip-compressed archive |
| `tar -xzvf archive.tar.gz` | Extract gzip-compressed archive |
| `tar -tvf archive.tar` | List contents of archive without extracting |
| `zip -r archive.zip dir/` | Create a zip archive |
| `unzip archive.zip` | Extract a zip archive |
| `gzip <file>` | Compress a file (.gz) |
| `gunzip <file.gz>` | Decompress a .gz file |
| `bzip2 <file>` | Compress using bzip2 |
| `xz <file>` | Compress using xz (high ratio) |

---

## 10. User & Group Management

| Command | Description |
|---|---|
| `useradd <name>` | Create new user |
| `useradd -m <name>` | Create user with home directory |
| `userdel <name>` | Delete user |
| `userdel -r <name>` | Delete user and their home directory |
| `passwd <name>` | Set/change user password |
| `usermod -aG <group> <user>` | Add user to a group |
| `groupadd <name>` | Create new group |
| `groupdel <name>` | Delete a group |
| `groups <user>` | Show groups a user belongs to |
| `su <user>` | Switch user |
| `su -` | Switch to root with root's environment |
| `sudo <cmd>` | Run command as superuser |
| `sudo -i` | Start interactive root shell |
| `visudo` | Safely edit sudoers file |
| `chage -l <user>` | Show password aging info |

---

## 11. Disk & Filesystem Management

| Command                | Description                                  |
| ---------------------- | -------------------------------------------- |
| `fdisk -l`             | List disk partitions                         |
| `parted`               | Advanced partition editor                    |
| `mkfs.ext4 /dev/sdX1`  | Format partition with ext4 filesystem        |
| `mount /dev/sdX1 /mnt` | Mount a filesystem                           |
| `umount /mnt`          | Unmount a filesystem                         |
| `mount -a`             | Mount all filesystems in `/etc/fstab`        |
| `blkid`                | Show block device UUIDs and filesystem types |
| `df -hT`               | Show disk usage with filesystem type         |
| `fsck /dev/sdX1`       | Check/repair filesystem                      |
| `lvcreate / lvextend`  | Logical Volume Manager commands              |
| `swapon -s`            | Show active swap space                       |
| `mkswap /dev/sdX2`     | Create swap partition                        |

---

## 12. Permissions, Links & Special Files

| Command | Description |
|---|---|
| `ln <target> <linkname>` | Create a hard link |
| `ln -s <target> <linkname>` | Create a symbolic (soft) link |
| `readlink -f <link>` | Resolve full path of a symlink |
| `chattr +i <file>` | Make file immutable (even root can't delete) |
| `lsattr <file>` | List file attributes |

---

## 13. Job Scheduling & Automation

| Command | Description |
|---|---|
| `crontab -e` | Edit current user's cron jobs |
| `crontab -l` | List current user's cron jobs |
| `crontab -r` | Remove all cron jobs for user |
| `at <time>` | Schedule a one-time task |
| `systemctl list-timers` | List active systemd timers |
| `watch -n 5 <cmd>` | Run command repeatedly every 5 seconds |

---

## 14. System Services (systemd)

| Command | Description |
|---|---|
| `systemctl start <service>` | Start a service |
| `systemctl stop <service>` | Stop a service |
| `systemctl restart <service>` | Restart a service |
| `systemctl status <service>` | Show service status |
| `systemctl enable <service>` | Enable service at boot |
| `systemctl disable <service>` | Disable service at boot |
| `systemctl daemon-reload` | Reload systemd manager configuration |
| `journalctl -u <service>` | View logs for a specific service |
| `journalctl -f` | Follow system logs in real time |
| `journalctl -xe` | Show recent logs with explanations |
| `service <name> status` | Legacy service status check |

---

## 15. Environment & Shell Configuration

| Command | Description |
|---|---|
| `echo $VAR` | Print value of environment variable |
| `export VAR=value` | Set environment variable for session |
| `env` | List all environment variables |
| `printenv` | Print environment variables |
| `alias ll='ls -la'` | Create command shortcut |
| `unalias ll` | Remove an alias |
| `source ~/.bashrc` | Reload shell configuration file |
| `echo $PATH` | Show executable search path |
| `set -x` | Enable debug mode in shell script |
| `type <command>` | Show how a command would be interpreted |
| `exec <cmd>` | Replace current shell with command |

---

## 16. Input/Output Redirection & Pipes

| Command | Description |
|---|---|
| `cmd > file` | Redirect output to file (overwrite) |
| `cmd >> file` | Redirect output to file (append) |
| `cmd < file` | Use file as input |
| `cmd 2> file` | Redirect error output to file |
| `cmd &> file` | Redirect both stdout and stderr |
| `cmd1 \| cmd2` | Pipe output of cmd1 into cmd2 |
| `tee file` | Write output to file and stdout simultaneously |
| `cmd1 && cmd2` | Run cmd2 only if cmd1 succeeds |
| `cmd1 \|\| cmd2` | Run cmd2 only if cmd1 fails |
| `cmd1 ; cmd2` | Run cmd2 regardless of cmd1's result |
| `/dev/null` | Discard output (e.g., `cmd > /dev/null`) |

---

## 17. Advanced / Power-User Commands

| Command | Description |
|---|---|
| `strace <cmd>` | Trace system calls made by a program |
| `ltrace <cmd>` | Trace library calls made by a program |
| `lsof` | List open files and the processes using them |
| `lsof -i :8080` | Show which process is using a port |
| `ulimit -a` | Show resource limits for the shell |
| `chroot /path` | Change root directory for a process |
| `dd if=/dev/sda of=backup.img` | Low-level data copy/disk imaging |
| `rsync -avz --delete src/ dst/` | Sync with deletion of extra files at destination |
| `tcpdump -i eth0` | Capture network packets on an interface |
| `iptables -A INPUT -p tcp --dport 22 -j ACCEPT` | Add firewall rule |
| `sysctl -a` | Show all kernel parameters |
| `sysctl -w net.ipv4.ip_forward=1` | Modify kernel parameter at runtime |
| `mkfifo <name>` | Create a named pipe (FIFO) |
| `screen` | Terminal multiplexer for persistent sessions |
| `tmux` | Modern terminal multiplexer |
| `bc` | Command-line calculator |
| `xxd <file>` | Hex dump of a file |
| `od -c <file>` | Octal/character dump of a file |
| `md5sum <file>` | Compute MD5 checksum |
| `sha256sum <file>` | Compute SHA-256 checksum |
| `openssl genrsa -out key.pem 2048` | Generate RSA private key |
| `ssh-keygen -t rsa` | Generate SSH key pair |
| `crontab -u <user> -e` | Edit another user's cron jobs (as root) |
| `parallel` | Run commands in parallel from input |
| `strings <file>` | Extract printable strings from binary file |
| `objdump -d <binary>` | Disassemble a binary file |
| `gdb <binary>` | GNU debugger for programs |
| `valgrind <cmd>` | Detect memory leaks/errors in programs |
| `dtrace` / `bpftrace` | Advanced kernel tracing tools |
| `unshare` | Run program in new namespace (containers basics) |
| `nsenter` | Enter existing namespaces (e.g., debug containers) |
| `cgroups (cgcreate, cgexec)` | Control resource allocation per process group |

---

## 18. Shell Scripting Essentials

| Command/Syntax | Description |
|---|---|
| `#!/bin/bash` | Shebang line — defines script interpreter |
| `chmod +x script.sh` | Make a script executable |
| `./script.sh` | Run a script |
| `if [ condition ]; then ... fi` | Conditional statement |
| `for i in list; do ... done` | For loop |
| `while [ condition ]; do ... done` | While loop |
| `case $var in ... esac` | Case/switch statement |
| `function name() { ... }` | Define a function |
| `$1, $2, $@` | Positional script arguments |
| `read var` | Read user input into a variable |
| `exit 0` | Exit script with status code |
| `trap 'cmd' SIGINT` | Catch signals and run a command |

---

## Quick Reference: Most-Used Daily Commands

```bash
ls -la          # list all files with details
cd /path        # change directory
pwd             # current directory
cp -r src dst   # copy folder
mv old new      # rename/move
rm -rf dir      # delete folder
grep -rn "x" .  # search recursively with line numbers
find . -name "*.log"
df -h           # disk usage
top / htop      # process monitor
ssh user@host   # remote login
sudo systemctl restart nginx
```

---

*Tip: Use `man <command>` or `<command> --help` anytime to see full documentation and options for any command.*
