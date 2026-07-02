# Service User Creation without Home Directory

## Description

This task demonstrates how to create a Linux service user without a home directory. Service users are created to run applications or system services rather than allowing interactive user logins. Since these accounts are used only by services, they do not require a home directory or personal files. This improves security, reduces unnecessary resource usage, and follows the Principle of Least Privilege (PoLP).

---

# What is a Service User?

A **service user** is a Linux user account created specifically to run a service or application. Unlike regular users, service users are not intended for human login.

Common examples include:

- Apache (`www-data`)
- Nginx (`www-data`)
- MySQL (`mysql`)
- PostgreSQL (`postgres`)
- Jenkins (`jenkins`)
- Docker (`docker` on some distributions)

A service user is responsible for:

- Running an application or service
- Owning application files
- Reading configuration files
- Writing logs

A service user is **not** intended for:

- Interactive login
- Running personal commands
- Storing personal files

---

# Why Create a Service User without a Home Directory?

A regular user typically has a home directory such as:

```text
/home/sakshi
```

where personal files, shell configurations, and documents are stored.

A service user does not require any of these because no person logs in using that account.

Creating unnecessary home directories:

- Wastes disk space
- Creates unused files
- Increases the attack surface
- Makes system administration more difficult

For these reasons, service users are usually created **without a home directory**.

---

# Hands-on

## Step 1: Create a Service User without a Home Directory

```bash
sudo useradd -M serviceuser
```

### Explanation

| Option | Description |
|---------|-------------|
| `-M` | Do not create a home directory. |

---

## Step 2: Verify the User

```bash
grep serviceuser /etc/passwd
```

Example Output

```text
serviceuser:x:1003:1004::/home/serviceuser:/bin/sh
```

Although `/home/serviceuser` appears in the account information, the actual directory is **not created** because the `-M` option was used.

---

## Step 3: Verify the Home Directory

```bash
ls -ld /home/serviceuser
```

Expected Output

```text
ls: cannot access '/home/serviceuser': No such file or directory
```

This confirms that no home directory exists.

---

## Step 4: Display User Information

```bash
id serviceuser
```

Example Output

```text
uid=1003(serviceuser) gid=1004(serviceuser) groups=1004(serviceuser)
```

Explanation:

- **UID** = Unique User ID
- **GID** = Primary Group ID
- **Groups** = Groups assigned to the user

---

# Recommended Production Command

In production environments, a service account is usually created using:

```bash
sudo useradd -r -M -s /usr/sbin/nologin myservice
```

### Explanation

| Option | Description |
|---------|-------------|
| `-r` | Creates a system user. |
| `-M` | Does not create a home directory. |
| `-s /usr/sbin/nologin` | Prevents interactive login. |

Verify:

```bash
grep myservice /etc/passwd
```

Example Output

```text
myservice:x:998:998::/home/myservice:/usr/sbin/nologin
```

---

# Verify the Home Directory

```bash
ls -ld /home/myservice
```

Expected Output

```text
ls: cannot access '/home/myservice': No such file or directory
```

---

# Verify the Assigned Shell

```bash
getent passwd myservice
```

Example Output

```text
myservice:x:998:998::/home/myservice:/usr/sbin/nologin
```

---

# Test Login

```bash
su - myservice
```

Expected Output

```text
This account is currently not available.
```

This confirms that interactive login is disabled.

---

# Understanding How a Service User Works

A common question is:

> **If a service user cannot log in, how does it perform its tasks?**

The answer is simple:

**The service user does not log in. The operating system starts the service using that user's identity.**

For example, consider a custom application called **MyApp**.

Instead of a person logging in as `appuser`, the service manager (`systemd`) starts the application.

```
Root/Systemd
      │
      ▼
Starts MyApp Service
      │
      ▼
Application runs as appuser
      │
      ▼
Reads configuration files
Writes log files
Accesses application data
```

No interactive login is required.

---

# Real-World Example

Suppose Apache is installed on the server.

You normally start Apache using:

```bash
sudo systemctl start apache2
```

You do **not** log in as the Apache user.

Instead, Linux starts the Apache process using the Apache service account.

Verify:

```bash
ps -ef | grep apache2
```

Example Output

```text
root       3117   1  0 05:59 ? 00:00:00 /usr/sbin/apache2 -k start
www-data   3119 3117 0 05:59 ? 00:00:00 /usr/sbin/apache2 -k start
www-data   3120 3117 0 05:59 ? 00:00:00 /usr/sbin/apache2 -k start
```

Explanation:

- The **root** process starts Apache.
- Apache then creates worker processes.
- The worker processes run as **www-data**.
- No one logs in as **www-data**.

---

# Why Use a Non-Interactive Shell?

A service account should not allow users to log in.

Using:

```text
/usr/sbin/nologin
```

prevents interactive access.

Attempting to log in:

```bash
su - myservice
```

returns:

```text
This account is currently not available.
```

However, services can still run normally because Linux starts them under the service user's identity.

---

# Command Summary

| Command | Description |
|----------|-------------|
| `useradd -M serviceuser` | Create a user without a home directory. |
| `useradd -r -M -s /usr/sbin/nologin myservice` | Create a secure system service account. |
| `grep username /etc/passwd` | Verify the user account. |
| `id username` | Display UID, GID, and group information. |
| `getent passwd username` | Display account details. |
| `ls -ld /home/username` | Verify whether the home directory exists. |
| `su - username` | Test interactive login. |

---

# Why This Is Important

- Improves system security.
- Prevents unauthorized interactive logins.
- Eliminates unnecessary home directories.
- Reduces disk usage.
- Follows the Principle of Least Privilege (PoLP).
- Provides dedicated identities for services.

---

# Real-World Use Cases

- Apache Web Server
- Nginx Web Server
- Jenkins Automation Server
- MySQL Database Server
- PostgreSQL Database
- Backup Services
- Monitoring Agents
- CI/CD Pipelines
- Custom Enterprise Applications

---

# Key Takeaways

- A service user is created to run services, not for human login.
- The `-M` option prevents the creation of a home directory.
- The `-r` option creates a system user.
- The `/usr/sbin/nologin` shell blocks interactive logins.
- Services run under the service user's identity, even though the user cannot log in.
- Always verify the user configuration after creation.

---

# Author

**Sakshi Upadhyay**

DevOps | Linux | Docker | Kubernetes | AWS | CI/CD