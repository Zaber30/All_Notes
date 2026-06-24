# 1. Find Chromium executable path (best method)

```
which chromium
```

or

```
which chromium-browser
```

### 📌 Output examples:

- `/snap/bin/chromium` → Snap version
- `/usr/bin/chromium-browser` → APT version

---

# 🔍 2. See full binary location

```
whereis chromium
```

### Example output:

```
chromium: /snap/bin/chromium /usr/lib/chromium-browser
```

---

# 📦 3. If installed via Snap (most common in Ubuntu)

Check:

```
snap list chromium
```

### Actual installation path:

```
/snap/chromium/current/
```

Binary is usually:

```
/snap/bin/chromium
```

---

# 📁 4. Explore full Snap app files

```
ls /snap/chromium/
```

You may see:

- `current/`
- version folders like `1/`, `2/`, etc.

---

# 🧠 5. Check running process path

If Chromium is running:

```
ps aux | grep chromium
```

Or:

```
readlink -f $(which chromium)
```