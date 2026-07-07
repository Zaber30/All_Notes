`docker run` command has **100+ flags**. Below are the most important and commonly used ones, organized by category, with concise descriptions.

---

# General Flags

|Flag|Long Flag|Description|
|---|---|---|
|`-d`|`--detach`|Run the container in the background (detached mode).|
|`-i`|`--interactive`|Keep standard input (STDIN) open even if not attached.|
|`-t`|`--tty`|Allocate a pseudo-terminal (TTY).|
|`-a`|`--attach`|Attach to STDIN, STDOUT, or STDERR.|
|`--detach-keys`||Override the key sequence used to detach from the container.|
|`--name`||Assign a custom name to the container.|
|`--rm`||Automatically remove the container after it exits.|
|`--cidfile`||Store the container ID in a file.|

---

# Environment Variables

|Flag|Long Flag|Description|
|---|---|---|
|`-e`|`--env`|Set an environment variable inside the container.|
|`--env-file`||Load environment variables from a file.|

---

# Working Directory & User

|Flag|Long Flag|Description|
|---|---|---|
|`-w`|`--workdir`|Set the working directory inside the container.|
|`-u`|`--user`|Run the container as a specific user or UID/GID.|
|`--userns`||Configure the user namespace mode.|
|`--group-add`||Add supplementary groups for the container process.|

---

# Networking

|Flag|Long Flag|Description|
|---|---|---|
|`--network`||Connect the container to a specific network.|
|`--network-alias`||Assign an additional network hostname (alias).|
|`--ip`||Assign a specific IPv4 address.|
|`--ip6`||Assign a specific IPv6 address.|
|`--mac-address`||Assign a custom MAC address.|
|`--hostname`||Set the container's hostname.|
|`--dns`||Specify a DNS server.|
|`--dns-option`||Configure DNS resolver options.|
|`--dns-search`||Specify DNS search domains.|
|`--add-host`||Add custom hostname-to-IP mappings (like `/etc/hosts`).|
|`--link`||Link containers together (legacy feature).|
|`--publish-all`||Publish all exposed ports to random host ports.|

---

# Port Mapping

|Flag|Long Flag|Description|
|---|---|---|
|`-p`|`--publish`|Map a host port to a container port.|
|`-P`|`--publish-all`|Publish all exposed ports automatically.|
|`--expose`||Expose a container port without publishing it to the host.|

---

# Storage & Mounts

|Flag|Long Flag|Description|
|---|---|---|
|`-v`|`--volume`|Mount a volume or bind mount.|
|`--mount`||Advanced syntax for mounting volumes, bind mounts, or tmpfs.|
|`--tmpfs`||Mount a temporary filesystem (stored in memory).|
|`--volume-driver`||Specify the volume driver.|
|`--volumes-from`||Mount volumes from another container.|
|`--read-only`||Mount the container's root filesystem as read-only.|

---

# Resource Limits

|Flag|Long Flag|Description|
|---|---|---|
|`--memory`||Limit the amount of memory the container can use.|
|`--memory-reservation`||Set a soft memory limit.|
|`--memory-swap`||Limit total memory plus swap usage.|
|`--memory-swappiness`||Control the container's swap behavior.|
|`--kernel-memory`||Limit kernel memory usage (deprecated on many systems).|
|`--cpus`||Limit the number of CPUs available to the container.|
|`--cpu-shares`||Set the relative CPU weight.|
|`--cpu-period`||Configure the CPU Completely Fair Scheduler (CFS) period.|
|`--cpu-quota`||Set the CPU CFS quota.|
|`--cpuset-cpus`||Restrict the container to specific CPU cores.|
|`--cpuset-mems`||Restrict the container to specific memory nodes (NUMA).|
|`--blkio-weight`||Set the block I/O weight.|
|`--device-read-bps`||Limit read bandwidth for a device.|
|`--device-write-bps`||Limit write bandwidth for a device.|
|`--device-read-iops`||Limit read I/O operations per second.|
|`--device-write-iops`||Limit write I/O operations per second.|
|`--pids-limit`||Limit the number of processes in the container.|
|`--oom-kill-disable`||Disable the Out-Of-Memory killer.|
|`--oom-score-adj`||Adjust the OOM score for the container.|
|`--shm-size`||Set the size of `/dev/shm` (shared memory).|
|`--ulimit`||Set resource limits such as open files or processes.|

---

# Restart Policy

|Flag|Long Flag|Description|
|---|---|---|
|`--restart`||Configure the container restart policy (`no`, `always`, `on-failure`, `unless-stopped`).|

---

# Security

|Flag|Long Flag|Description|
|---|---|---|
|`--privileged`||Give the container extended privileges.|
|`--cap-add`||Add Linux capabilities.|
|`--cap-drop`||Remove Linux capabilities.|
|`--security-opt`||Set security options (e.g., AppArmor, SELinux, Seccomp).|
|`--userns`||Configure user namespace mode.|
|`--sysctl`||Set kernel parameters inside the container.|
|`--no-new-privileges`||Prevent the process from gaining additional privileges.|

---

# Devices

|Flag|Long Flag|Description|
|---|---|---|
|`--device`||Add a host device to the container.|
|`--device-cgroup-rule`||Add device access rules.|
|`--gpus`||Make GPUs available to the container (supported environments).|

---

# Logging

|Flag|Long Flag|Description|
|---|---|---|
|`--log-driver`||Select the logging driver.|
|`--log-opt`||Configure logging driver options.|

---

# Health Checks

|Flag|Long Flag|Description|
|---|---|---|
|`--health-cmd`||Command used to check container health.|
|`--health-interval`||Time between health checks.|
|`--health-timeout`||Maximum time a health check may run.|
|`--health-retries`||Number of consecutive failures before marking unhealthy.|
|`--health-start-period`||Grace period before starting health checks.|
|`--health-start-interval`||Interval between checks during the startup period.|
|`--no-healthcheck`||Disable image-defined health checks.|

---

# Process Control

|Flag|Long Flag|Description|
|---|---|---|
|`--entrypoint`||Override the image's default entrypoint.|
|`--init`||Run a small init process as PID 1 to handle signals and child processes.|
|`--stop-signal`||Specify the signal sent to stop the container.|
|`--stop-timeout`||Time to wait before forcefully killing the container.|
|`--sig-proxy`||Forward received signals to the container process.|

---

# Labels

|Flag|Long Flag|Description|
|---|---|---|
|`-l`|`--label`|Add metadata (key-value pairs) to the container.|
|`--label-file`||Load labels from a file.|

---

# Console & Terminal

|Flag|Long Flag|Description|
|---|---|---|
|`--console-size`||Set the console size (rows and columns).|
|`--tty`||Allocate a pseudo-terminal.|
|`--interactive`||Keep STDIN open.|

---

# Miscellaneous

|Flag|Long Flag|Description|
|---|---|---|
|`--platform`||Specify the target platform (e.g., `linux/amd64`, `linux/arm64`).|
|`--pull`||Control whether Docker pulls the image before running.|
|`--runtime`||Specify the OCI runtime to use.|
|`--cgroup-parent`||Set the parent cgroup.|
|`--cgroupns`||Configure the cgroup namespace.|
|`--ipc`||Configure the IPC namespace.|
|`--pid`||Configure the PID namespace.|
|`--uts`||Configure the UTS namespace (hostname/domain).|