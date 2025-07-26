To configure **Wi-Fi on Fedora 42 Server Edition**, which **does not come with a GUI**, you'll need to use the **command line**. Here are the methods based on whether you're using `nmcli`, `nmtui`, or a manual config.

---

## 🛰️ Wi-Fi Setup on Fedora 42 Server (Headless)

---

### ✅ 1. **Check Wi-Fi Device**

Run:

```bash
nmcli device status
```

Look for a device with `wifi` type, e.g.:

```
DEVICE       TYPE      STATE        CONNECTION
wlp2s0       wifi      disconnected  --
```

---

### ✅ 2. **Enable NetworkManager (if not already)**

Fedora uses **NetworkManager** by default. Ensure it's running:

```bash
sudo systemctl enable --now NetworkManager
```

---

### ✅ 3. **List Available Wi-Fi Networks**

```bash
nmcli dev wifi list
```

---

### ✅ 4. **Connect to Wi-Fi Using `nmcli`**

```bash
nmcli dev wifi connect "YourSSID" password "YourPassword"
```

* Replace `"YourSSID"` with the network name.
* Replace `"YourPassword"` with your actual Wi-Fi password.

If successful, it will automatically create a profile and connect.

---

### 🧠 Tip: Save Connection Automatically

Once connected, it saves to a profile:

```bash
nmcli connection show
```

To make it autoconnect:

```bash
nmcli connection modify "YourSSID" connection.autoconnect yes
```

---

### 🧑‍💻 Alternative: Use `nmtui` (Text-based UI)

If you prefer a UI-like interface in terminal:

1. Run:

   ```bash
   sudo dnf install -y NetworkManager-tui
   ```

2. Then:

   ```bash
   sudo nmtui
   ```

3. Choose:

   * **Activate a connection**
   * Select your Wi-Fi and enter password

---

### ✅ 5. **Verify Connection**

```bash
nmcli device status
```

Check if the Wi-Fi interface is "connected".

Then:

```bash
ip a
```

See if it got an IP address.

---

### 🚫 If Wi-Fi Not Detected

Try:

```bash
lspci | grep -i network
lsusb | grep -i wifi
```

Then check drivers:

```bash
sudo dmesg | grep firmware
```

You may need to install drivers:

```bash
sudo dnf install linux-firmware
```

---

