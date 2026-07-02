# Linux User Setup with Non-Interactive Shell

## Description

This task demonstrates how to create a Linux user with a non-interactive shell. A non-interactive shell prevents users from logging into the system while still allowing the account to exist for running services, applications, or owning files. This is a common security practice for service accounts such as Apache, Nginx, MySQL, Jenkins, and other system services.

---

## Objective

- Create a Linux user with a non-interactive shell.
- Verify the assigned login shell.
- Test that the user cannot log in.
- Understand the purpose of non-interactive user accounts.

---

## Prerequisites

- Ubuntu/Debian Linux
- Sudo privileges

---

# Step 1: View Available Login Shells

Display all valid login shells configured on the system.

```bash
cat /etc/shells
```

Example Output

```text
/bin/sh
/usr/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/usr/sbin/nologin
```

> **Note:** If `/usr/sbin/nologin` is not available on your system, you can use `/bin/false` as an alternative.

---

# Step 2: Create a User with a Non-Interactive Shell

Create a user named **appuser** with `/usr/sbin/nologin` as the login shell.

```bash
sudo useradd -m -s /usr/sbin/nologin appuser
```

### Command Explanation

- `-m` : Creates the user's home directory.
- `-s` : Specifies the login shell.
- `/usr/sbin/nologin` : Prevents interactive login.

---

# Step 3: Verify User Creation

Check the user's entry in the password file.

```bash
grep appuser /etc/passwd
```

Example Output

```text
appuser:x:1002:1002::/home/appuser:/usr/sbin/nologin
```

The last field indicates that the user's login shell is set to **/usr/sbin/nologin**.

---

# Step 4: Display User Information

View the user's UID, GID, and group membership.

```bash
id appuser
```

Example Output

```text
uid=1002(appuser) gid=1002(appuser) groups=1002(appuser)
```

---

# Step 5: Test User Login

Attempt to switch to the newly created user.

```bash
su - appuser
```

Expected Output

```text
This account is currently not available.
```

This confirms that the user cannot obtain an interactive shell.

---

# Step 6: Verify Assigned Shell

You can also verify the configured shell using:

```bash
getent passwd appuser
```

Example Output

```text
appuser:x:1002:1002::/home/appuser:/usr/sbin/nologin
```

---

# Optional: Change the Shell to Bash

If interactive login is required later, change the shell to `/bin/bash`.

```bash
sudo usermod -s /bin/bash appuser
```

Verify the change.

```bash
grep appuser /etc/passwd
```

Example Output

```text
appuser:x:1002:1002::/home/appuser:/bin/bash
```

---

# Command Summary

| Command | Description |
|----------|-------------|
| `cat /etc/shells` | Displays all valid login shells. |
| `useradd -m -s /usr/sbin/nologin appuser` | Creates a user with a home directory and a non-interactive shell. |
| `grep appuser /etc/passwd` | Verifies the user's login shell. |
| `id appuser` | Displays user ID and group information. |
| `su - appuser` | Tests whether the user can log in. |
| `getent passwd appuser` | Displays the user's account information. |
| `usermod -s /bin/bash appuser` | Changes the user's login shell to Bash. |

---

# Why Use a Non-Interactive Shell?

- Prevents unauthorized interactive logins.
- Improves system security.
- Protects service accounts from misuse.
- Follows the Principle of Least Privilege (PoLP).
- Reduces the attack surface on production servers.

---

# Real-World Use Cases

- Apache service account
- Nginx service account
- MySQL database user
- Jenkins automation user
- Backup service accounts
- CI/CD service accounts
- Monitoring tools and agents

---

# Key Takeaways

- A non-interactive shell prevents users from accessing the command line.
- `/usr/sbin/nologin` is the preferred shell for service accounts.
- `/bin/false` can be used as an alternative if `nologin` is unavailable.
- Always verify the assigned shell after creating the user.
- Non-interactive accounts enhance security without affecting service functionality.

---

# Author

**Sakshi Upadhyay**

DevOps | Linux | Docker | Kubernetes | AWS | CI/CD