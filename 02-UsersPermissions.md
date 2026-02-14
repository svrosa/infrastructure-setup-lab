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
```bash
cat /etc/passwd
```
**Example Entries:**  
root:x:0:0:root:/root:/bin/bash  
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin  
srosa:x:1000:1000:Sandro Rosa:/home/srosa:/bin/bash  
sandroadmin:x:1001:1001:,,,:/home/sandroadmin:/bin/bash

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

## /etc/shadow Analysis
```bash
sudo cat /etc/shadow
```
Stores encrypted password hashes and password aging policies.
Only readable by root.

**Example Entries:**  
username:$y$j9T$abc123hash...:19000:0:99999:7:::

### Field Structure
`username:password_hash:last_change:min:max:warn:inactive:expire:reserved`

**Security Insight:**
Separation of /etc/passwd and /etc/shadow prevents exposure of password hashes to normal users.

## /etc/shadow Permissions

**Owner:** root  
**Group:** shadow  
**Permissions:** -rw-r----- 
### Breakdown
Owner (root) → rw- (read + write)  
Group (shadow) → r-- (read only)  
Others → --- (no access)

**Security Insight:**
Only root and the shadow group can read password hashes.
Normal users cannot access this file.

## /etc/group Analysis
```bash
cat /etc/group
```

**Example Entries:**  
sudo:x:27:srosa,sandroadmin  
root:x:0

### Field Structure
`group_name:x:GID:user1,user2,user3`

**Purpose:**
Defines group memberships and group IDs.  

**Security Insight:**
Users in the sudo group can perform privilege escalation via sudo.
Users are not members of the shadow group, preventing direct access to password hashes.
