# Day 03 – Linux Commands Cheat Sheet

---

## 1. Process Management Commands

- `ps aux` – Show all running processes
- `top` – Real-time CPU and memory usage
- `htop` – Interactive process viewer 
- `kill PID` – Kill a process by PID
- `kill -9 PID` – Force kill a process
- `pkill nginx` – Kill process by name
- `pgrep nginx` – Get PID of a process
- `uptime` – Check system running time and load
- `free -h` – Check memory usage
- `vmstat` – View memory and CPU stats

---



## 2. File System & Disk Commands

- `ls` – List files and directories
- `ls -lh` – List files with size
- `pwd` – Show current directory
- `cd /path` – Change directory
- `touch file.txt` – Create empty file
- `mkdir test` – Create directory
- `rm file.txt` – Delete file
- `rm -rf folder` – Delete folder forcefully
- `df -h` – Check disk space usage
- `du -sh folder` – Check folder size

---

## 3. Log & File Viewing Commands

- `cat file` – View file content
- `less file` – View file page by page
- `tail file` – View last lines of file
- `tail -f app.log` – Live log monitoring
- `grep error file` – Search word in file
- `wc -l file` – Count lines in file

---


## 4. Networking & Troubleshooting Commands

- `ping google.com` – Check network connectivity
- `ip addr` – View IP address details
- `ss -tuln` – View open ports and services
- `curl http://example.com` – Test HTTP response
- `netstat -tulnp` – Check listening ports 

---


## 5. systemd & Service Commands

- `systemctl status nginx` – Check service status
- `systemctl start nginx` – Start service
- `systemctl stop nginx` – Stop service
- `systemctl restart nginx` – Restart service
- `journalctl -u nginx` – View service logs

---