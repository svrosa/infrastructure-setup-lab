## whoami 
**Output:** 
srosa 
**Purpose:**
Used to display the username of the current user.

## id
**Output:**
uid=1000(srosa) gid=1000(srosa) groups=1000(srosa), 27(sudo)  
**Security Insight:**
User is part of the sudo group, allowing privilege escalation via sudo.
Root login is not used directly.  
**Purpose:**
Displays user ID(UID), primary group ID(GID), group memberships.
Confirms if user has sudo privileges.

## uname -a
**Output:**
Linux cloudlab-01 6.8.0-100-generic #100-Ubuntu SMP PREEMPT_DYNAMIC Tue Jan 13 16:40:06 UTC 2026 x86_64 x86_64 x86_64 GNU/Linux  
**Purpose:**
Used to display all system information.

## hostname
**Output:**
cloudlab-01  
**Purpose:**
Used to show current hostname.

## ip a
**Output (Summary):**  
Primary Interface: ens3  
State: UP  
IPv4 Address: 192.168.109.128/24 (NAT - Private Network)  
Loopback Address: 127.0.0.1/8  
**Purpose:**
Displays network interfaces and IP configuration.  
Used to verify connectivity and confirm assigned IP address.

## df -h
**Output (Summary):**  
Root Filesystem (/):    
- Total Size: 24G    
- Used: 6.4G    
- Available: 16G    
- Usage: 29%    

Boot Partition (/boot):    
- Size: 2G    

Storage Model:    
- LVM (Logical Volume Manager) enabled.  

Storage Configuration  

- Virtual Disk Size: 50GB    
- Root Logical Volume (/): 24GB allocated    
- Free Space in LVM: ~24GB (available for expansion)
- LVM is enabled for flexible storage management.  
**Purpose:**
Displays disk usage and mounted filesystems.  
Used to verify available storage and confirm LVM configuration.

## free -h
**Output (Summary):**  
- Total RAM: 3.8Gi  
- Used: 483Mi  
- Available: 3.3Gi  
- Swap: Disable (0B)  
**Purpose:**  
Displays memory usage.
The "Available" field shows usable memory without swapping.

## top
**Output (Snapshot Summary):**
Load Average: 0.07, 0.02, 0.00
CPU Usage: ~1% user, ~1% system (mostly idle)
Memory Usage: 478.8 used / 3.8Gi
Tasks: 212 total (1 running, 211 sleeping, 0 zobmie)
**Purpose:**
Displays real-time system resource usage including CPU, memory, and process state.
Used to monitor system health and detect abnormal behavior.
