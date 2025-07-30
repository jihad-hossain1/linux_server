Here is a complete PM2 setup and usage guide written in **Markdown (`.md`) format**, specifically tailored for your setup (Node.js projects using `npm run start`, located in `/home/jihad/projects/b2c`).

---

### 📄 `pm2-setup-guide.md`

````markdown
# PM2 Setup Guide for Node.js Projects on Fedora Linux Server

This guide explains how to install, configure, and auto-start multiple Node.js projects using PM2 on a Fedora Linux Server.

---

## 🧱 Project Structure

Your project directories:

- **Frontend**: `/home/jihad/projects/b2c/frontend`
- **Backend**: `/home/jihad/projects/b2c/backend`
- Both use: `npm run start`

---

## ✅ Step 1: Install PM2 Globally

```bash
npm install -g pm2
````

---

## ✅ Step 2: Start Your Projects with PM2

```bash
# Start the frontend project
pm2 start npm --name b2c-frontend --prefix /home/jihad/projects/b2c/frontend -- run start

# Start the backend project
pm2 start npm --name b2c-backend --prefix /home/jihad/projects/b2c/backend -- run start
```

---

## ✅ Step 3: Save the PM2 Process List

```bash
pm2 save
```

This saves the currently running apps into `~/.pm2/dump.pm2`.

---

## ✅ Step 4: Setup PM2 to Run on Boot

### Run `pm2 startup`:

```bash
pm2 startup
```

You will get a command like this:

```bash
sudo env PATH=$PATH:/home/jihad/.nvm/versions/node/v22.17.1/bin /home/jihad/.nvm/versions/node/v22.17.1/lib/node_modules/pm2/bin/pm2 startup systemd -u jihad --hp /home/jihad
```

### Copy and run the full command it gives you:

```bash
sudo env PATH=$PATH:/home/jihad/.nvm/versions/node/v22.17.1/bin /home/jihad/.nvm/versions/node/v22.17.1/lib/node_modules/pm2/bin/pm2 startup systemd -u jihad --hp /home/jihad
```

### Save the process list again:

```bash
pm2 save
```

---

## ✅ Step 5: Verify Setup

Check that the systemd service was created and is running:

```bash
sudo systemctl status pm2-jihad
```

You should see:

```
Active: active (running)
```

---

## ✅ Step 6: Test Reboot

```bash
sudo reboot
```

After reboot, log in and verify that your apps are running:

```bash
pm2 list
```

---

## 🛠 PM2 Useful Commands

| Command                    | Description               |
| -------------------------- | ------------------------- |
| `pm2 list`                 | Show all running apps     |
| `pm2 logs`                 | View combined logs        |
| `pm2 logs b2c-frontend`    | View frontend logs        |
| `pm2 restart b2c-frontend` | Restart app               |
| `pm2 stop b2c-backend`     | Stop backend app          |
| `pm2 delete b2c-frontend`  | Remove app from PM2       |
| `pm2 save`                 | Save current process list |
| `pm2 startup`              | Generate startup service  |

---

## 📦 Optional: Ecosystem File (Alternative to manual start)

Create `/home/jihad/projects/b2c/ecosystem.config.js`:

```js
module.exports = {
  apps: [
    {
      name: "b2c-frontend",
      script: "npm",
      args: "run start",
      cwd: "/home/jihad/projects/b2c/frontend",
    },
    {
      name: "b2c-backend",
      script: "npm",
      args: "run start",
      cwd: "/home/jihad/projects/b2c/backend",
    },
  ],
};
```

Start both apps with:

```bash
cd /home/jihad/projects/b2c
pm2 start ecosystem.config.js
pm2 save
```

---

## ✅ Done!

Your Node.js projects are now:

* Managed by PM2
* Restarted if they crash
* Auto-started on system boot

---

```

