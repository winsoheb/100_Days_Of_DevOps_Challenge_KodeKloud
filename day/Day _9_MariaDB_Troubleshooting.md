# Day 9: MariaDB Troubleshooting

**Difficulty**: 🟡 Intermediate  
**Category**: Troubleshooting / Database  
**Estimated Time**: 15-20 minutes  

---

## 🎯 Objective

Troubleshoot and restore the **MariaDB** database service on the **Database Server**. The application is currently unable to connect to the database, and the service appears to be down.

---

## 📖 Scenario

The **Nautilus App deployment team** has reported an issue with the application. Upon investigation, they found that the application cannot connect to the backend database.
As a DevOps Engineer, you have been assigned to identify the root cause and bring the **MariaDB** service back online.

---

## 📋 Prerequisites

- Access to the Database Server (SSH)
- `sudo` privileges
- Basic knowledge of systemd and Linux file permissions

---

## 🛠 Tools & Technologies

- Linux (CentOS/RHEL)
- MariaDB (Database Server)
- systemctl
- journalctl

---

## 🔧 Implementation Steps

### Step 1: Check Service Status

First, check the current status of the MariaDB service to confirm it is down.

```sh
sudo systemctl status mariadb
```

If the service is in a **failed** state, check the logs for error messages.

### Step 2: Analyze Logs

View the system logs tailored for the MariaDB service to find the specific error.

```sh
sudo journalctl -xeu mariadb.service
```

OR check the specific MariaDB log file (if it exists and is configured):

```sh
cat /var/log/mariadb/mariadb.log
```

**Common Error Indicators:**
- `Permissions denied`
- `Can't create/write to file ... (Errcode: 13)`
- `Job for mariadb.service failed`

### Step 3: Fix Ownership / Permissions

A common issue in this scenario is incorrect ownership of the MariaDB data directory. The `mysql` user requires ownership of its data directories.

Check current ownership:

```sh
ls -ld /var/lib/mysql
```

If it is owned by `root` or another user, fix it by changing ownership to `mysql`:

```sh
sudo chown -R mysql:mysql /var/lib/mysql
```

Also, ensure the runtime directory exists and has correct permissions if mentioned in logs:

```sh
sudo chown -R mysql:mysql /var/run/mariadb
```

### Step 4: Start the Service

After fixing the permissions, attempt to start the service again.

```sh
sudo systemctl start mariadb
```

### Step 5: Verify the Fix

Check the status again to ensure it is **active (running)**.

```sh
sudo systemctl status mariadb
```

Optionally, try logging into the database to ensure it's responsive:

```sh
mysql -u root -p
```

---

## ✅ Verification Checklist

- [ ] Service `mariadb` is running (`active`)
- [ ] No errors in `systemctl status mariadb`
- [ ] `/var/lib/mysql` is owned by `mysql:mysql`
- [ ] Database is accessible via client

---

## 🔧 Troubleshooting

**Issue**: Service still fails after permission fix.
**Solution**:
- Check if another process is using the port (3306).
- Check `my.cnf` configuration for syntax errors.
- Ensure disk space is not full (`df -h`).

---

## 🧠 Key Learnings

- **Logs are your best friend**: Always read `journalctl` or `/var/log` to find the root cause.
- **File Permissions**: Database services are sensitive to file ownership; always run as the dedicated user (e.g., `mysql`).
- **Systemd**: Understanding how to control and debug system services is crucial for DevOps.

---

## os Best Practices

- **Monitoring**: Set up alerts for service state changes.
- **Automation**: Use Ansible or Puppet to ensure correct permissions are always set.
- **Backups**: Regular configuration and data backups are essential before troubleshooting.