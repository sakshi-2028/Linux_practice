# Custom Apache User Setup in Linux

## Description

This task demonstrates how to create a custom user for the Apache HTTP Server instead of using the default user. Running Apache with a dedicated user improves security, simplifies permission management, and follows the Principle of Least Privilege (PoLP).

---

## Objective

- Create a custom user for Apache.
- Create a dedicated group for the Apache user.
- Configure Apache to run using the custom user and group.
- Restart the Apache service.
- Verify that Apache is running under the new user.

---

## Prerequisites

- Ubuntu/Debian Linux
- Apache HTTP Server installed
- Sudo privileges

---

## Step 1: Create a Custom User

Create a new user named **webuser**.

```bash
sudo useradd webuser
```

Verify the user:

```bash
cat /etc/passwd | grep webuser
```

Example Output:

```text
webuser:x:1001:1001:,,,:/home/webuser:/bin/bash
```

---

## Step 2: Create a Group

Create a dedicated group for Apache.

```bash
sudo groupadd webgroup
```

---

## Step 3: Add User to the Group

```bash
sudo usermod -aG webgroup webuser
```

Verify:

```bash
groups webuser
```

Example Output:

```text
webuser : webuser users webgroup
```

---

## Step 4: Configure Apache

Open the Apache environment configuration file:

```bash
sudo nano /etc/apache2/envvars
```

Locate the following lines:

```bash
export APACHE_RUN_USER=www-data
export APACHE_RUN_GROUP=www-data
```

Replace them with:

```bash
export APACHE_RUN_USER=webuser
export APACHE_RUN_GROUP=webgroup
```

Save the file and exit.

---

## Step 5: Restart Apache

Restart the Apache service to apply the changes.

```bash
sudo systemctl restart apache2
```

---

## Step 6: Verify the Configuration

Check the running Apache processes.

```bash
ps -ef | grep apache2
```

Example Output:

```text
root        3117       1  0 05:59 ?        00:00:00 /usr/sbin/apache2 -k start
webuser     3119    3117  0 05:59 ?        00:00:00 /usr/sbin/apache2 -k start
webuser     3120    3117  0 05:59 ?        00:00:00 /usr/sbin/apache2 -k start
```

The Apache worker processes should run as **webuser**, confirming that the configuration has been applied successfully.

---

## Command Summary

| Command | Description |
|----------|-------------|
| `sudo useradd webuser` | Creates a new Linux user. |
| `sudo groupadd webgroup` | Creates a new group. |
| `sudo usermod -aG webgroup webuser` | Adds the user to the specified group. |
| `sudo nano /etc/apache2/envvars` | Opens the Apache environment configuration file. |
| `sudo systemctl restart apache2` | Restarts the Apache service. |
| `ps -ef \| grep apache2` | Verifies the Apache process owner. |

---

## Why Use a Custom Apache User?

- Improves system security.
- Limits Apache's permissions.
- Simplifies file and directory permission management.
- Reduces the impact of security vulnerabilities.
- Follows the Principle of Least Privilege (PoLP).

---

## Real-World Use Cases

- Hosting multiple web applications on a single server.
- Enterprise production environments.
- Shared hosting platforms.
- Secure DevOps deployments.
- Organizations with strict security policies.

---

## Key Takeaways

- Apache can be configured to run with a custom non-privileged user.
- Worker processes should run as the configured custom user.
- Always restart Apache after modifying its configuration.
- Verify the running user to ensure the changes are effective.

---

## Author

**Sakshi Upadhyay**

DevOps | Linux | Docker | Kubernetes | AWS | CI/CD