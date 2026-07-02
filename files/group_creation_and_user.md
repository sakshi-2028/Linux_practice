# Group Creation and User Assignment in Linux

## Description

This task demonstrates how to create a Linux group, create users, and assign users to the group. Groups are used to simplify permission management by allowing multiple users to share the same access rights to files and directories. This approach improves security and makes system administration more efficient.

---

## Objective

- Create a new Linux group.
- Create user accounts.
- Assign users to the group.
- Verify group membership.
- Demonstrate group-based permissions on a directory.

---

## Prerequisites

- Ubuntu/Debian Linux
- Sudo privileges

---

# Step 1: View Existing Groups

Display all groups available on the system.

```bash
cat /etc/group
```

To search for a specific group:

```bash
grep developers /etc/group
```

---

# Step 2: Create a New Group

Create a group named **developers**.

```bash
sudo groupadd developers
```

Verify the group:

```bash
grep developers /etc/group
```

Example Output

```text
developers:x:1002:
```

---

# Step 3: Create New Users

Create two users.

```bash
sudo useradd -m rahul
sudo useradd -m sakshi
```

Verify the users:

```bash
cat /etc/passwd | grep rahul
cat /etc/passwd | grep sakshi
```

Example Output

```text
rahul:x:1002:1003::/home/rahul:/bin/bash
sakshi:x:1003:1004::/home/sakshi:/bin/bash
```

---

# Step 4: Assign Users to the Group

Add **rahul** to the **developers** group.

```bash
sudo usermod -aG developers rahul
```

Add **sakshi** to the same group.

```bash
sudo usermod -aG developers sakshi
```

> **Note:**  
> `-a` = Append the user to additional groups.  
> `-G` = Specify one or more supplementary groups.

---

# Step 5: Verify Group Membership

Check Rahul's groups.

```bash
groups rahul
```

Example Output

```text
rahul : rahul developers
```

Check Sakshi's groups.

```bash
groups sakshi
```

Example Output

```text
sakshi : sakshi developers
```

You can also use:

```bash
id rahul
```

Example Output

```text
uid=1002(rahul) gid=1003(rahul) groups=1003(rahul),1002(developers)
```

---

# Step 6: Verify the Group File

Display the group information.

```bash
grep developers /etc/group
```

Example Output

```text
developers:x:1002:rahul,sakshi
```

---

# Step 7: Create a Shared Directory

Create a project directory.

```bash
sudo mkdir /opt/project
```

Assign the directory to the **developers** group.

```bash
sudo chown :developers /opt/project
```

Provide read, write, and execute permissions to the owner and group.

```bash
sudo chmod 770 /opt/project
```

Verify the permissions.

```bash
ls -ld /opt/project
```

Example Output

```text
drwxrwx---  root developers  /opt/project
```

Now all users in the **developers** group can access the directory.

---

# Command Summary

| Command | Description |
|----------|-------------|
| `groupadd developers` | Creates a new Linux group. |
| `useradd -m username` | Creates a new user with a home directory. |
| `usermod -aG developers username` | Adds the user to the specified group without removing existing memberships. |
| `groups username` | Displays all groups assigned to the user. |
| `id username` | Displays the user's UID, GID, and group memberships. |
| `grep developers /etc/group` | Displays information about the group. |
| `mkdir /opt/project` | Creates a shared project directory. |
| `chown :developers /opt/project` | Assigns group ownership of the directory. |
| `chmod 770 /opt/project` | Grants full access to the owner and group only. |

---

# Why Group Management is Important

- Simplifies permission management.
- Reduces administrative effort.
- Allows multiple users to share access to resources.
- Improves system security.
- Supports the Principle of Least Privilege (PoLP).

---

# Real-World Use Cases

- Development teams working on shared projects.
- DevOps engineers managing deployment directories.
- Shared application folders.
- Log management directories.
- CI/CD pipeline workspaces.
- Enterprise Linux environments.

---

# Key Takeaways

- A **group** is a collection of users used for managing permissions.
- Users can belong to multiple groups.
- The `groupadd` command creates a new group.
- The `usermod -aG` command safely adds users to additional groups.
- Group ownership simplifies access control for shared resources.
- Always verify user and group assignments after making changes.

---

# Author

**Sakshi Upadhyay**

DevOps | Linux | Docker | Kubernetes | AWS | CI/CD