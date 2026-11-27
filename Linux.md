1. Explain Linux architecture. What is a Linux shell? Explain different types of shells.   ---> https://linuxsimply.com/linux-basics/introduction/architecture-of-linux-operating-system/
2. What is systemd in Linux? Explain systemctl and journalctl.   
**Answer** - systemd is the system and service manager. It is usually the first process started after the Linux kernel boots and runs with: PID 1
```
# Check overall system state
systemctl is-system-running
# Check a service
systemctl status nginx
# Start a service
sudo systemctl start nginx
# Stop a service
sudo systemctl stop nginx
# Restart a service
sudo systemctl restart nginx
# Reload configuration without fully restarting
sudo systemctl reload nginx
# Enable service at boot
sudo systemctl enable nginx
# Disable service at boot
sudo systemctl disable nginx
# Check whether a service is running
systemctl is-active nginx
# Check whether a service is enabled
systemctl is-enabled nginx
# List failed services
systemctl --failed

```
**start vs enable** 
systemctl start nginx starts Nginx immediately, but does not guarantee it starts after reboot.  
systemctl enable nginx configures Nginx to start automatically during boot, but may not start it immediately.  

4. 
5. 
6. What is a shebang line in a script?
**Answer** A shebang line is the first line of a script that tells Linux or Unix which interpreter should execute it.  
```
#!/bin/bash
```
- #! is called the shebang
- /bin/bash is the interpreter used to run the script
```
#!/bin/bash
#!/usr/bin/env python3
#!/usr/bin/env node
```
Without shebang, we need to specify the interpreter exclusively
```
bash script.sh
```

8. Explain the Linux file hierarchy.
 https://linuxhandbook.com/linux-directory-structure/

10. Explain file permissions in Linux.
11. What is the difference between yum and rpm?
12. What is a cron job? How do you schedule a job?
13. 
14. Explain different filtering commands: sed, awk, find, grep.
15. Explain user management commands in Linux.
16. How do you check permissions for a file?
17. Difference between a soft link and a hard link?
18. How do you take a compressed backup of a file in Linux?
19. Explain different networking commands in Linux.
20. How do you check if a service port is open?
21. How do you check the status of a service process?
22. What are disk monitoring commands in Linux?
23. How do you handle a corrupted file system? Explain the magic block error.
24. What are different package management commands in Linux?
25. How do you kill a particular process in Linux?
26. How to search inside a file?
27. How to check if a port is listening?
28. Why we used shell script in linux?
