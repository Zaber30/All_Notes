You add a user to the `docker` group with the `usermod` command.

### Step 1: Add `zaber` to the `docker` group

```
sudo usermod -aG docker zaber
```

### What does this command mean?

```
usermod│├── -a  → Append (don't remove existing groups)├── -G  → Specify supplementary group(s)└── docker → Group to add
```

So it means:

> **Add (`-a`) the user `zaber` to the supplementary group (`-G`) `docker`.**

---

### Step 2: Verify the system configuration

Run:

```
getent group docker
```

Expected output:

```
docker:x:973:zaber
```

This means the system has recorded that `zaber` is a member of the `docker` group.

---

### Step 3: Refresh your session

The new group membership is **not applied to your current login session** automatically.

Choose one of these:

- Log out and log back in.
- Reboot:

```
sudo reboot
```

---

### Step 4: Verify your current session

After logging back in:

```
groups
```

Expected output:

```
zaber adm cdrom sudo docker dip plugdev users lpadmin lxd
```

Notice `docker` is now listed.

---

### Step 5: Test Docker

Now you should be able to run:

```
docker ps
```

without `sudo`.

---

## Why use `-aG` instead of just `-G`?

Suppose you currently belong to:

```
zaber adm sudo lpadmin
```

If you run:

```
sudo usermod -G docker zaber
```

you **replace** all supplementary groups with only `docker`.

Result:

```
zaber docker
```

You would lose membership in groups like `sudo`, `adm`, and `lpadmin`.

Using:

```
sudo usermod -aG docker zaber
```

**appends** `docker` while keeping your existing group memberships.

This is why `-aG` is the recommended form.