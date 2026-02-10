# Users & Permissions

## Creating a New User
```bash
sudo adduser <username>
sudo usermod -aG sudo <username>
```
**Purpose:**
Creates a new local user and grants sudo privileges by adding the user to the sudo group.  
**Security Insight:**
Privilege escalation is controlled via group memberships.
Users should not log in as root directly.

## Switching User Context
```bash
su - <username>
```
**Purpose:**
Switches to the specified user and loads their login environment.

## Verifying Group Membership
```bash
id <username>
```
**or**
```bash
id
```
**Purpose:**
Displays the user's  UID, primary GID, and group memberships to confirm sudo access.

## /etc/passwd Analysis

**Example Entries:**  
root:x:0:0:root:/root:/bin/bash  
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin  
[username]:x:1000:1000:Sandro Rosa:/home/srosa:/bin/bash  
[username]:x:1001:1001:,,,:/home/sandroadmin:/bin/bash

### Field Structure
`username:x:UID:GID:comment:home_directory:shell`

### 1) Root UID

UID 0 = unlimited system privileges.  
Any account with UID 0 has full system control.

### 2) UID Ranges

- UID 0 = root
- UID 1–999 = system/service accounts
- UID 1000+ = regular human/user accounts

### 3) Login Shell

- `/bin/bash` = interactive login allowed
- `/usr/sbin/nologin` or `/bin/false` = login disabled

System accounts use restricted shells to prevent interactive access.

### Command Used for Cleaner View

```bash
awk -F: '{print $1, $3, $7}' /etc/passwd
```
**Shows:** username UID shell  
**Purpose:** Cleaner View.
