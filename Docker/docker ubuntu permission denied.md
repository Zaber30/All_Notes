The root cause was that **your user process did not have permission to access Docker's Unix socket**.

Let's break it down.

## How Docker works

Docker has two main components:

1. **Docker daemon (`dockerd`)** – runs as `root` and manages containers.
2. **Docker CLI (`docker`)** – the command you type.

When you run:

```
docker ps
```

the CLI does **not** run containers itself. It sends a request to the Docker daemon through a Unix socket:

```
/var/run/docker.sock
```

---

## Your socket permissions

You showed:

```
srw-rw---- 1 root docker ... /var/run/docker.sock
```

This means:

- Owner: `root`
- Group: `docker`

Permissions:

```
rw- rw- ---
```

So:

- ✅ `root` can access it.
- ✅ Members of the `docker` group can access it.
- ❌ Everyone else is denied.

---

## Your user's groups

Initially, you had:

```
zaber adm cdrom sudo dip plugdev users lpadmin lxd
```

Notice that **`docker` was missing**.

Therefore, when you ran:

```
docker pull ...
```

Linux checked:

> "Is `zaber` root?" → **No**

> "Is `zaber` in the `docker` group?" → **No**

Result:

```
permission denied while trying to connect to the docker API at unix:///var/run/docker.sock
```

---

## Then you added yourself

You ran:

```
sudo usermod -aG docker zaber
```

Now the system database showed:

```
docker:x:973:zaber
```

This means your account was correctly added to the `docker` group.

---

## Why it still didn't work immediately

Linux determines your group memberships **when you log in**.

Your current shell was created **before** you were added to the `docker` group, so it continued using the old group list.

Only after:

- logging out and logging back in,
- rebooting,
- or starting a new login shell,

does your shell receive the updated group membership.

---

## Why `sudo docker ...` worked

When you use:

```
sudo docker pull ...
```

the Docker CLI runs as `root`.

Since `root` owns `/var/run/docker.sock`, it always has permission to access the Docker daemon.

---

### Summary

1. Docker daemon was running. ✅
2. `/var/run/docker.sock` was owned by `root:docker`. ✅
3. Your user wasn't initially in the `docker` group. ❌
4. Linux denied access to the socket. ❌
5. After adding your user to the `docker` group and starting a new login session, you should be able to use Docker without `sudo`. ✅

This is a common Linux permissions mechanism: access to files, directories, and sockets is controlled by the owner, group, and permission bits. Docker uses that same model to control who can communicate with its daemon.