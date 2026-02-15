# System Hardening
## Update & Upgrade
**Inside the VM:**
```bash
sudo apt update
sudo apt upgrade -y
```

**Security Insight:**
Keeping packages updated reduces exposure to known CVEs(Common Vulnerabilities and Exposures) and privilege escalation vulnerabilities.

## Install UFW (Firewall)

**Inside the VM:**  
`sudo apt install ufw -y `  
**Check status:**  
`sudo ufw status verbose `  
It will likely say: inactive.

**Now configure:**
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw enable
```
Confirm:  
`sudo ufw status verbose `  

You should see:
- Default deny incoming
- SSH allowed
- Firewall active

**Security Insight:**  
Default deny model = zero trust baseline.  
Only explicitly allowed services are reachable.  
This mirrors cloud security groups behavior.

### Validate Firewall
**Run:**  
`sudo ufw status numbered `

This shows rule numbering.  
**Then:**    
`sudo iptables -L `  

This shows underlying firewall rules.  
Important for understanding that UFW is a frontend to iptables.

## Install fail2ban

**Inside the VM:**  
`sudo apt install fail2ban -y `  
Check:  
`sudo systemctl status fail2ban `  
**Enable at boot:**  
`sudo systemctl enable fail2ban `  

**Purpose:** 

Even though password auth is disabled, fail2ban:
- Monitors logs
- Detects brute force attempts
- Automatically bans IP addresses performing suspicous activity.  

This simulates intrusion detection behavior in cloud environments.

### Verify Jail
**Run:**  
`sudo fail2ban-client status `  
Should see:  
`jail list: sshd `  
**Then:**  
`sudo fail2ban-client status sshd `  

It shows:
- Currently banned
- Total banned

Likely 0 since its a local lab.
