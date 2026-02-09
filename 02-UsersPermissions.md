# Users & Permissions

## Creating a New User

```bash
sudo adduser <username>
sudo usermod -aG sudo <username>
```
**Purpose:**
Creates a new local user and grants sudo privileges by adding the user to the sudo group.
