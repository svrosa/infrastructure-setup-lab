# Install & Configure SSH

## Verify if SSH is installed:  
`
sudo systemctl status ssh
` 

## Enable SSH if needed:  
`
sudo systemctl enable ssh
`
  
This ensures SSH starts automatically after reboot. 

**Security Insight:**  
Remote access services should always be verified and controlled. Should never assume a service is running securely by default.

## Verify if its listening on port 22:  
`
sudo ss -tulnp | grep ssh
`  

**Example Entries:**  
tcp LISTEN 0 0000 0.0.0.0:22 0.0.0.0:* users:(("sshd",pid=920, fd=3),("systemd", pid=1, fd=98))  
tcp LISTEN 0 0000 [::]:22 [::]:* users:(("sshd",pid=920, fd=4),("systemd", pid=1, fd=99))

**SSH is:**
- Running 
- Listening on port 22 
- Bound to all IPv4 interfaces (0.0.0.0) 
- Bound to all IPv6 interfaces ([::]) 
- Managed by systemd 
  
This is a fully active SSH daemon.  

**Security Insight:**  
Binding to 0.0.0.0 means that SSH accepts connections on all network interfaces, in a real cloud environments this must be restricted either via firewall or security group rules.  

## Check SSH
`
sudo nano /etc/ssh/sshd_config
`  

**Example Entries:**  
PermitRootLogin prohibit-password  
PasswordAuthentication yes  
PubkeyAuthentication yes  

**What it means:**
- PermitRootLogin prohibit-password
  - Root cannot log in with a password
  - Root can log in with SSH keys (if configured)

- PasswordAuthentication yes
  - Password logins are allowed (we’ll disable later)

- PubkeyAuthentication yes
  - Key-based login is allowed (good)  

**Security Insight:**  
Default configs favors usability and simplicity. Hardening requires disabling insecure authentication methods.  

### Verify SSH works from Windows host
Safety step so we dont lock ourselves out.
**Get VM IP:**
`
ip a | grep inet
`  

**Example Entries:**  
inet 192.168.109.xxx/xx metric 100 brd 192.xxx.xxx.255 scope global dynamic ens33

Look for the ens33 IPv4 192.168.109.xxx.

**Windows Powershell:**
`
ssh <username>@192.xxx.xxx.xxx
`  
Might see something like this:  
```bash
The authenticity of host cant be established...
Are you sure you want to continue connecting (yes/no)?
```  
Type yes, you'll be prompted enter your password.

**It this works:**  
You'll be inside VM via SSH.
Meaning:
- SSH is properly configured
- Network works
- You can safely harden withou locking yourself out. **Most Important!**

## Harden SSH
### Generate SSH Key on Windows:
In powershell we run:  
`ssh-keygen -t ed25519 `  
For lab simplicity we leave default location and no passphrase.  
**This creates at:**
```bash
C:\Users\<user>\.ssh\id_ed25519  
C:\Users\<user>\.ssh\id_ed25519.pub
```
**Security Insight:**  
The private key remains on the client, only the public key is copied to the server.  

### Copy Public Key to VM
In powershell we run:  
`type $env:USERPROFILE\.ssh\id_ed25519.pub `  
Copy the full output.  
`ssh-ed25519 <key.........> <user>@DESKTOP-NAME `  
**Inside the SSH session (Powershell):**  
```bash
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
```  
Paste the key and save it.  
**Then we set permissions:**  
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```
**Security Insight:**  
SSH requires strict permissions. If `.ssh` or `authorized_keys` is too permissive, the authentication will fail.  

**Once the key is saved and pemissions are set. We test by open a new powershell window.**  
`ssh <user>@<vm_ip> `  
We should be able to login, without password being required.

## Disable Password Auth
**Inside VM, we run:**  
`sudo nano /etc/ssh/sshd_config `  
We find **PasswordAuthentication** and we change it to **no**, 
confirm that **PermitRootLogin** is set to **no** , also delete "#" so it is not commented.

### Restart SSH and Password test
**Restart SSH:**  
`sudo systemctl restart ssh `  
**In a new powershell window:**  
`ssh -o PreferredAuthentications=password <user>@<vm_ip> `  
We try to force password, it should fail.

**If it doesnt, we do:**
`sudo sshd -T | grep -E "passwordauthentication|permitrootlogin" `
It will probably say:
```bash
permitrootlogin no  
passwordauthentication yes
```
**This means SSH is overriding the settings somewhere.**

### Check/Fix Override
**Ubuntu often uses:**  
`/etc/ssh/sshd_config.d/ `  
So lets fix it.   
**Run:**  
`ls /etc/ssh/sshd_config.d/ `  
**Output:**    
`50-cloud-init.conf ` or something like this.     
**We look for culprits:**  
`sudo grep -Ri passwordauthentication /etc/ssh/ `  
Look for those that say: `PasswordAuthentication yes` , disregard any that have "#", thats a commented line.  
In this case, it will be the `50-cloud-init.conf `.  
**Run:**  
`sudo nano /etc/ssh/sshd_config.d/50-cloud-init.conf `, change PasswordAuthentication to no.  
Save and Exit.  
**Restart SSH:** `sudo systemctl restart ssh `.  
**Verify again:** `sudo sshd -T | grep passwordauthentication `.  
**Should say:** `passwordauthentication no `.  

## Final Checks
We try to force login with password again.  
**In Powershell:**  
`ssh -o PreferredAuthentications=password <user>@<vm_ip> `  
it should say:  
`<user>@<vm_ip>: Permission denied (publickey) `  

### Best-Practice Check
**On VM, we run:**  
`sudo sshd -T | grep -E "passwordauthentication|permitrootlogin|pubkeyauthentication" `  
**Should see:**
- passwordauthentication no  
- permitrootlogin no  
- pubkeyauthentication yes

## Security Summary
- SSH service is enabled and verified
- Key-based authentication configured
- Password authentication disabled
- Root login disabled
- Override configurations audited
- Remote access validated safely before and after hardening
