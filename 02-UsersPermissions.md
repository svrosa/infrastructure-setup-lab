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
