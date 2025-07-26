If you've forgotten the **username** for logging into your **Fedora 42 Server Edition (localhost)**, here are a few ways to **find or reset** it:

---

### ✅ **Option 1: Reboot into Single User Mode (Recovery Method)**

This gives you root access without needing to log in:

#### Steps:

1. **Reboot the system**.

2. At the GRUB menu:

   * Press `e` to **edit the boot entry**.

3. Find the line starting with:

   ```
   linux ...
   ```

4. At the end of that line, **append**:

   ```
   systemd.unit=rescue.target
   ```

5. Press `Ctrl + X` or `F10` to boot with this setting.

6. Now you’ll be dropped into a **root shell**.

---

### 🔍 **Find Existing Users**

From the root shell, you can list all users:

```bash
cut -d: -f1 /etc/passwd
```

Or more specifically:

```bash
awk -F: '$3 >= 1000 && $3 < 65534 {print $1}' /etc/passwd
```

> This will show normal users (not system accounts).

---

### 🔄 **Reset Password for a User**

Once you find the username (e.g., `john`), you can reset the password:

```bash
passwd john
```

---

### 💾 **Reboot Back to Normal Mode**

Once done:

```bash
reboot
```

---

### 🛠️ **If GRUB Is Password-Protected**

If you can’t access GRUB or it’s locked, you'll need **physical access** and possibly a **Live USB** to chroot into the system.

---

