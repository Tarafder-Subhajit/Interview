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
7. Explain the Linux file hierarchy.
8. Explain file permissions in Linux.
9. What is the difference between yum and rpm?
10. What is a cron job? How do you schedule a job?
11. 
12. Explain different filtering commands: sed, awk, find, grep.
13. Explain user management commands in Linux.
14. How do you check permissions for a file?
15. Difference between a soft link and a hard link?
16. How do you take a compressed backup of a file in Linux?
17. Explain different networking commands in Linux.
18. How do you check if a service port is open?
19. How do you check the status of a service process?
20. What are disk monitoring commands in Linux?
21. How do you handle a corrupted file system? Explain the magic block error.
22. What are different package management commands in Linux?
23. How do you kill a particular process in Linux?
24. How to search inside a file?
25. How to check if a port is listening?
26. Why we used shell script in linux?
