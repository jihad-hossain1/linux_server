Here is a complete and **production-ready installation and setup guide** for **PostgreSQL** and **pgAdmin 4 (Web Mode)** on **Fedora 42**.

---

# 📘 PostgreSQL + pgAdmin Web Installation Guide for Fedora 42

---

## 📦 PART 1: Install PostgreSQL

---

### ✅ Step 1.1: Install PostgreSQL server

```bash
sudo dnf install -y postgresql-server postgresql-contrib
```

---

### ✅ Step 1.2: Initialize PostgreSQL database cluster

```bash
sudo postgresql-setup --initdb
```

> This initializes the database in `/var/lib/pgsql/data`.

---

### ✅ Step 1.3: Start and Enable PostgreSQL service

```bash
sudo systemctl enable --now postgresql
```

Check status:

```bash
sudo systemctl status postgresql
```

---

### ✅ Step 1.4: Set password for `postgres` user

Switch to the postgres user:

```bash
sudo -i -u postgres
psql
```

Inside psql prompt:

```sql
ALTER USER postgres WITH PASSWORD 'your_secure_password';
\q
```

Exit postgres shell:

```bash
exit
```

---

### ✅ Step 1.5: Enable password authentication

Edit `pg_hba.conf`:

```bash
sudo nano /var/lib/pgsql/data/pg_hba.conf
```

Find and **update** the following lines:

```diff
local   all             all                                     trust
host    all             all             127.0.0.1/32            trust
host    all             all             ::1/128                 trust
```

---

### ✅ Step 1.6: Allow TCP/IP connections

Edit `postgresql.conf`:

```bash
sudo nano /var/lib/pgsql/data/postgresql.conf
```

Uncomment and set:

```conf
listen_addresses = 'localhost'
```

> For remote access later: `listen_addresses = '*'`

---

### ✅ Step 1.7: Restart PostgreSQL to apply changes

```bash
sudo systemctl restart postgresql
```

---

## 🧪 Verify PostgreSQL

Try:

```bash
psql -U postgres -h 127.0.0.1 -d postgres -W
```

---

## 🌐 PART 2: Install pgAdmin 4 (Web Mode)

---

### ✅ Step 2.1: Add pgAdmin YUM repository

```bash
sudo dnf install -y https://ftp.postgresql.org/pub/pgadmin/pgadmin4/yum/pgadmin4-fedora-repo-2-1.noarch.rpm
```

---

### ✅ Step 2.2: Install pgAdmin Web

```bash
sudo dnf install -y pgadmin4-web
```

---

### ✅ Step 2.3: Configure pgAdmin Web

Run the setup script:

```bash
sudo /usr/pgadmin4/bin/setup-web.sh
```

It will prompt:

* **Email** (admin login)
* **Password** (for the web UI)

---

### ✅ Step 2.4: Start and Enable Web Server

pgAdmin runs via Apache (httpd):

```bash
sudo systemctl enable --now httpd
```

Check:

```bash
sudo systemctl status httpd
```

---

### ✅ Step 2.5: Open firewall port (optional for browser access)

```bash
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --reload
```

---

### ✅ Step 2.6: Access pgAdmin in browser

Open:

```
http://127.0.0.1/pgadmin4
```

Login with the email and password you set during setup.

---

## 🧩 Add PostgreSQL Server in pgAdmin

1. Click **"Add New Server"**
2. Go to **General** tab:

   * Name: `Local PostgreSQL`
3. Go to **Connection** tab:

   * Host: `127.0.0.1`
   * Port: `5432`
   * Username: `postgres`
   * Password: `your_secure_password`

Click **Save** → You’re connected!

---

## 📋 Appendix

### 🔁 Restart Services

```bash
sudo systemctl restart postgresql
sudo systemctl restart httpd
```

---

### 🔐 Create New Database

```bash
sudo -i -u postgres
psql
CREATE DATABASE mydb;
\q
exit
```

---

### 🌍 Optional Remote Access

#### 1. In `postgresql.conf`:

```conf
listen_addresses = '*'
```

#### 2. In `pg_hba.conf`:

```conf
host    all             all             0.0.0.0/0               md5
```

#### 3. Restart:

```bash
sudo systemctl restart postgresql
```

---

