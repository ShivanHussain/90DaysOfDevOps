# Day 05 – Linux Troubleshooting Runbook 

---

## Target Service

- Service Name: ssh
- Why chosen: Common service, easy to inspect, always running

---

## 1. Environment Basics

### Command 1
$ uname -a

Output:

Linux SH 6.14.0-37-generic #37~24.04.1-Ubuntu SMP PREEMPT_DYNAMIC Thu Nov 20 10:25:38 UTC 2 x86_64 x86_64 x86_64 GNU/Linux


---

### Command 2
$ lsb_release -a

Output:

Distributor ID:	Ubuntu
Description:	Ubuntu 24.04.3 LTS
Release:	24.04
Codename:	noble



---

## 2. Filesystem Sanity Check

### Command 3
$ mkdir /tmp/runbook-demo

Output:

Temporary folder created.

---

### Command 4
$ cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo

Output:

total 4

-rw-r--r-- 1 ubuntu ubuntu 221 Jan 28 10:01 hosts-copy

---

## 3. CPU & Memory Snapshot

### Command 5
$ top -b -n 1 | head -5

Output:

top - 10:03:34 up 3 min,  1 user,  load average: 0.00, 0.02, 0.00
Tasks: 116 total,   1 running, 115 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st 
MiB Mem :    914.2 total,    378.4 free,    345.7 used,    347.1 buff/cache     
MiB Swap:      0.0 total,      0.0 free,      0.0 used.    568.5 avail Mem 

---

### Command 6
$ free -h

Output:

               total        used        free      shared  buff/cache   available
Mem:           914Mi       345Mi       378Mi       2.7Mi       347Mi       568Mi
Swap:             0B          0B          0B

---

## 4. Disk / IO Check

### Command 7
$ df -h

Output:

Filesystem       Size  Used Avail Use% Mounted on
/dev/root         19G  2.2G   17G  12% /
tmpfs            458M     0  458M   0% /dev/shm
tmpfs            183M  872K  182M   1% /run
tmpfs            5.0M     0  5.0M   0% /run/lock
efivarfs         128K  3.8K  120K   4% /sys/firmware/efi/efivars
/dev/nvme0n1p16  881M   89M  730M  11% /boot
/dev/nvme0n1p15  105M  6.2M   99M   6% /boot/efi
tmpfs             92M   12K   92M   1% /run/user/1000

---

### Command 8
$ du -sh /var/log

Output:
120M    /var/log

---

## 5. Network Snapshot

### Command 9
$ ss -tulpn | grep ssh

Output:

LISTEN 0 128 0.0.0.0:22

---

### Command 10
$ ping -c 2 google.com

Output:

PING google.com (172.253.62.101) 56(84) bytes of data.
64 bytes from bc-in-f101.1e100.net (172.253.62.101): icmp_seq=1 ttl=109 time=1.78 ms
64 bytes from bc-in-f101.1e100.net (172.253.62.101): icmp_seq=2 ttl=109 time=1.80 ms

--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 1.780/1.790/1.801/0.010 ms

---

## 6. Logs Review

### Command 11
$ journalctl -u ssh -n 20 --no-pager

Output:

Jan 28 06:50:34 ip-172-31-10-242 ec2-instance-connect[9939]: Providing ssh key from EC2 Instance Connect with fingerprint: SHA256:TT205NVun39uez/TdrRM0r+tmCueEuC+ScL+YweavBo, request-id: 133a1cfc-cefc-448f-ab58-ac2b3c8e64c4, for IAM principal: arn:aws:iam::476186719188:root
Jan 28 06:50:35 ip-172-31-10-242 ec2-instance-connect[10056]: Querying EC2 Instance Connect keys for matching fingerprint: SHA256:TT205NVun39uez/TdrRM0r+tmCueEuC+ScL+YweavBo
Jan 28 06:50:35 ip-172-31-10-242 ec2-instance-connect[10088]: Providing ssh key from EC2 Instance Connect with fingerprint: SHA256:TT205NVun39uez/TdrRM0r+tmCueEuC+ScL+YweavBo, request-id: 133a1cfc-cefc-448f-ab58-ac2b3c8e64c4, for IAM principal: arn:aws:iam::476186719188:root
Jan 28 06:50:36 ip-172-31-10-242 sshd[9798]: Accepted publickey for ubuntu from 18.206.107.28 port 24394 ssh2: ED25519 SHA256:TT205NVun39uez/TdrRM0r+tmCueEuC+ScL+YweavBo
Jan 28 06:50:36 ip-172-31-10-242 sshd[9798]: pam_unix(sshd:session): session opened for user ubuntu(uid=1000) by ubuntu(uid=0)
Jan 28 07:11:32 ip-172-31-10-242 systemd[1]: Stopping ssh.service - OpenBSD Secure Shell server...
Jan 28 07:11:32 ip-172-31-10-242 sshd[8302]: Received signal 15; terminating.
Jan 28 07:11:32 ip-172-31-10-242 systemd[1]: ssh.service: Deactivated successfully.
Jan 28 07:11:32 ip-172-31-10-242 systemd[1]: Stopped ssh.service - OpenBSD Secure Shell server.
Jan 28 07:11:32 ip-172-31-10-242 systemd[1]: ssh.service: Consumed 1.357s CPU time, 1.6M memory peak, 0B memory swap peak.
-- Boot e97a19c156634972a7b14c867e2f1c6a --
Jan 28 09:59:52 ip-172-31-10-242 systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
Jan 28 09:59:52 ip-172-31-10-242 sshd[881]: Server listening on 0.0.0.0 port 22.
Jan 28 09:59:52 ip-172-31-10-242 sshd[881]: Server listening on :: port 22.
Jan 28 09:59:52 ip-172-31-10-242 systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
Jan 28 09:59:54 ip-172-31-10-242 ec2-instance-connect[991]: Querying EC2 Instance Connect keys for matching fingerprint: SHA256:7l92XuaK2KNuUUOQZc3I5OCEb6DFvLJarnj8wdw/yk8
Jan 28 09:59:54 ip-172-31-10-242 ec2-instance-connect[1023]: Providing ssh key from EC2 Instance Connect with fingerprint: SHA256:7l92XuaK2KNuUUOQZc3I5OCEb6DFvLJarnj8wdw/yk8, request-id: dbb3a856-9c4a-4d14-bcae-490dbe89717d, for IAM principal: arn:aws:iam::476186719188:root
Jan 28 09:59:55 ip-172-31-10-242 ec2-instance-connect[1140]: Querying EC2 Instance Connect keys for matching fingerprint: SHA256:7l92XuaK2KNuUUOQZc3I5OCEb6DFvLJarnj8wdw/yk8
Jan 28 09:59:55 ip-172-31-10-242 ec2-instance-connect[1172]: Providing ssh key from EC2 Instance Connect with fingerprint: SHA256:7l92XuaK2KNuUUOQZc3I5OCEb6DFvLJarnj8wdw/yk8, request-id: dbb3a856-9c4a-4d14-bcae-490dbe89717d, for IAM principal: arn:aws:iam::476186719188:root
Jan 28 09:59:55 ip-172-31-10-242 sshd[882]: Accepted publickey for ubuntu from 18.206.107.28 port 22118 ssh2: ED25519 SHA256:7l92XuaK2KNuUUOQZc3I5OCEb6DFvLJarnj8wdw/yk8
Jan 28 09:59:55 ip-172-31-10-242 sshd[882]: pam_unix(sshd:session): session opened for user ubuntu(uid=1000) by ubuntu(uid=0)

---

### Command 12
$ tail -n 20 /var/log/auth.log

Output:

2026-01-28T09:59:42.941606+00:00 ip-172-31-10-242 systemd-logind[563]: Watching system buttons on /dev/input/event2 (AT Translated Set 2 keyboard)
2026-01-28T09:59:42.941659+00:00 ip-172-31-10-242 polkitd[547]: Loading rules from directory /etc/polkit-1/rules.d
2026-01-28T09:59:42.941666+00:00 ip-172-31-10-242 polkitd[547]: Loading rules from directory /usr/share/polkit-1/rules.d
2026-01-28T09:59:42.941683+00:00 ip-172-31-10-242 polkitd[547]: Finished loading, compiling and executing 6 rules
2026-01-28T09:59:42.941690+00:00 ip-172-31-10-242 polkitd[547]: Acquired the name org.freedesktop.PolicyKit1 on the system bus
2026-01-28T09:59:52.704134+00:00 ip-172-31-10-242 sshd[881]: Server listening on 0.0.0.0 port 22.
2026-01-28T09:59:52.704352+00:00 ip-172-31-10-242 sshd[881]: Server listening on :: port 22.
2026-01-28T09:59:54.643649+00:00 ip-172-31-10-242 ec2-instance-connect[991]: Querying EC2 Instance Connect keys for matching fingerprint: SHA256:7l92XuaK2KNuUUOQZc3I5OCEb6DFvLJarnj8wdw/yk8
2026-01-28T09:59:54.678051+00:00 ip-172-31-10-242 ec2-instance-connect[1023]: Providing ssh key from EC2 Instance Connect with fingerprint: SHA256:7l92XuaK2KNuUUOQZc3I5OCEb6DFvLJarnj8wdw/yk8, request-id: dbb3a856-9c4a-4d14-bcae-490dbe89717d, for IAM principal: arn:aws:iam::476186719188:root
2026-01-28T09:59:55.591844+00:00 ip-172-31-10-242 ec2-instance-connect[1140]: Querying EC2 Instance Connect keys for matching fingerprint: SHA256:7l92XuaK2KNuUUOQZc3I5OCEb6DFvLJarnj8wdw/yk8
2026-01-28T09:59:55.622350+00:00 ip-172-31-10-242 ec2-instance-connect[1172]: Providing ssh key from EC2 Instance Connect with fingerprint: SHA256:7l92XuaK2KNuUUOQZc3I5OCEb6DFvLJarnj8wdw/yk8, request-id: dbb3a856-9c4a-4d14-bcae-490dbe89717d, for IAM principal: arn:aws:iam::476186719188:root
2026-01-28T09:59:55.953406+00:00 ip-172-31-10-242 sshd[882]: Accepted publickey for ubuntu from 18.206.107.28 port 22118 ssh2: ED25519 SHA256:7l92XuaK2KNuUUOQZc3I5OCEb6DFvLJarnj8wdw/yk8
2026-01-28T09:59:55.956369+00:00 ip-172-31-10-242 sshd[882]: pam_unix(sshd:session): session opened for user ubuntu(uid=1000) by ubuntu(uid=0)
2026-01-28T09:59:55.970188+00:00 ip-172-31-10-242 systemd-logind[563]: New session 1 of user ubuntu.
2026-01-28T09:59:56.007381+00:00 ip-172-31-10-242 (systemd): pam_unix(systemd-user:session): session opened for user ubuntu(uid=1000) by ubuntu(uid=0)
2026-01-28T10:05:01.972224+00:00 ip-172-31-10-242 CRON[1339]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
2026-01-28T10:05:01.983802+00:00 ip-172-31-10-242 CRON[1339]: pam_unix(cron:session): session closed for user root
2026-01-28T10:05:16.945851+00:00 ip-172-31-10-242 sudo:   ubuntu : TTY=pts/0 ; PWD=/tmp/runbook-demo ; USER=root ; COMMAND=/usr/bin/du -sh /var/log
2026-01-28T10:05:16.948999+00:00 ip-172-31-10-242 sudo: pam_unix(sudo:session): session opened for user root(uid=0) by ubuntu(uid=1000)
2026-01-28T10:05:16.950191+00:00 ip-172-31-10-242 sudo: pam_unix(sudo:session): session closed for user root

---
