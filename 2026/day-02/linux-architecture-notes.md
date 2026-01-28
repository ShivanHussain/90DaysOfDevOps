# Linux Architecture, Processes, and systemd


## Linux Architecture: 

## Application → Shell → Kernel → Hardware

Linux follows a layered architecture where each layer has a specific responsibility.

### Application

- User-level programs that perform tasks
- Examples: nginx, mysql, docker, python, vim, ls
- Applications cannot directly access hardware

### Shell

- Interface between user and operating system
- Takes user commands and interprets them
- Examples: bash, zsh, sh
- The shell sends this command to the kernel for execution.

Example:
```bash
ls
```

### Kernel

- Core component of Linux OS
- Manages CPU, memory, disk, and network
- Provides system calls for applications
- Communicates directly with hardware

### Hardware

- Physical system components
- CPU, RAM, Disk, Network Interface
- Only kernel can access hardware directly


## 2. Linux Processes

A process is a running instance of a program.

### Process Creation

- A user runs a command
- Kernel creates a process with a unique PID
- Process uses system resources

### Process States

- Running: Process is using CPU
- Sleeping: Waiting for resource or input
- Stopped: Process paused manually
- Zombie: Process completed but not removed


---


## 3. systemd

systemd is the init system used in modern Linux distributions.

- Starts services during system boot
- Manages running services
- Restarts failed services
- Handles system logs

systemd runs as the first process with PID 1.

---

## 4. Common systemd Commands

```bash
systemctl status nginx
systemctl start nginx
systemctl stop nginx
systemctl enable nginx
journalctl -u nginx
```

---

## 5. Daily Linux Commands

1. `ps` – View running processes
2. `top` – Monitor CPU and memory
3. `systemctl` – Manage services
4. `journalctl` – View logs
5. `kill` – Terminate a process

---

