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
