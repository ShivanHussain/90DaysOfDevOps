# Day 04 – Linux Practice (Processes & Services)

---

## 1. Process Checks

### Command 1
$ ps aux | head -5

Output:
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0  23984 15216 ?        Ss   14:11   0:02 /sbin/init splash
root           2  0.0  0.0      0     0 ?        S    14:11   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        S    14:11   0:00 [pool_workqueue_release]
root           4  0.0  0.0      0     0 ?        I<   14:11   0:00 [kworker/R-rcu_gp]


---

### Command 2
$ pgrep sshd

Output:
742


---

## 2. Service Checks (Using systemctl)

### Command 3
$ systemctl status docker

Output:
● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-01-28 14:11:24 IST; 1h 6min ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 1901 (dockerd)
      Tasks: 17
     Memory: 94.0M (peak: 96.9M)
        CPU: 1.189s
     CGroup: /system.slice/docker.service
             └─1901 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Jan 28 14:11:24 SH dockerd[1901]: time="2026-01-28T14:11:24.455907742+05:30" level=warning msg="Error (Unable to complete atomic operation, key modified) deleting object [endpoint_count b11>
Jan 28 14:11:24 SH dockerd[1901]: time="2026-01-28T14:11:24.511835027+05:30" level=info msg="Loading containers: done."
Jan 28 14:11:24 SH dockerd[1901]: time="2026-01-28T14:11:24.527066126+05:30" level=info msg="Docker daemon" commit="28.2.2-0ubuntu1~24.04.1" containerd-snapshotter=false storage-driver=over>
Jan 28 14:11:24 SH dockerd[1901]: time="2026-01-28T14:11:24.527326534+05:30" level=info msg="Initializing buildkit"
Jan 28 14:11:24 SH dockerd[1901]: time="2026-01-28T14:11:24.529250754+05:30" level=warning msg="CDI setup error /etc/cdi: failed to monitor for changes: no such file or directory"
Jan 28 14:11:24 SH dockerd[1901]: time="2026-01-28T14:11:24.529268176+05:30" level=warning msg="CDI setup error /var/run/cdi: failed to monitor for changes: no such file or directory"
Jan 28 14:11:24 SH dockerd[1901]: time="2026-01-28T14:11:24.540065519+05:30" level=info msg="Completed buildkit initialization"
Jan 28 14:11:24 SH dockerd[1901]: time="2026-01-28T14:11:24.547600192+05:30" level=info msg="Daemon has completed initialization"
Jan 28 14:11:24 SH systemd[1]: Started docker.service - Docker Application Container Engine.
Jan 28 14:11:24 SH dockerd[1901]: time="2026-01-28T14:11:24.547683990+05:30" level=info msg="API listen on /run/docker.sock"


---

### Command 4

$ systemctl list-units --type=service | head -5

Output:
  UNIT                                                                                      LOAD   ACTIVE SUB     DESCRIPTION
  accounts-daemon.service                                                                   loaded active running Accounts Service
  alsa-restore.service                                                                      loaded active exited  Save/Restore Sound Card State
  apparmor.service                                                                          loaded active exited  Load AppArmor profiles
  apport.service                                                                            loaded active exited  automatic crash report generation

---

## 3. Log Checks

### Command 5
$ journalctl -u ssh --no-pager | tail -5

Output:

Jan 28 14:11:24 SH dockerd[1901]: time="2026-01-28T14:11:24.529268176+05:30" level=warning msg="CDI setup error /var/run/cdi: failed to monitor for changes: no such file or directory"
Jan 28 14:11:24 SH dockerd[1901]: time="2026-01-28T14:11:24.540065519+05:30" level=info msg="Completed buildkit initialization"
Jan 28 14:11:24 SH dockerd[1901]: time="2026-01-28T14:11:24.547600192+05:30" level=info msg="Daemon has completed initialization"
Jan 28 14:11:24 SH systemd[1]: Started docker.service - Docker Application Container Engine.
Jan 28 14:11:24 SH dockerd[1901]: time="2026-01-28T14:11:24.547683990+05:30" level=info msg="API listen on /run/docker.sock"


---

### Command 6

$ tail -n 20 /var/log/syslog

Output:

2026-01-28T15:19:56.394729+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-72 noise=9999 txrate=13000
2026-01-28T15:19:59.408620+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-75 noise=9999 txrate=13000
2026-01-28T15:20:02.409880+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-73 noise=9999 txrate=26000
2026-01-28T15:20:05.416611+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-73 noise=9999 txrate=19500
2026-01-28T15:20:08.434408+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-73 noise=9999 txrate=13000
2026-01-28T15:20:11.444372+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-76 noise=9999 txrate=13000
2026-01-28T15:20:11.468100+05:30 SH systemd[1]: Starting sysstat-collect.service - system activity accounting tool...
2026-01-28T15:20:11.479542+05:30 SH systemd[1]: sysstat-collect.service: Deactivated successfully.
2026-01-28T15:20:11.480077+05:30 SH systemd[1]: Finished sysstat-collect.service - system activity accounting tool.
2026-01-28T15:20:14.455637+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-73 noise=9999 txrate=13000
2026-01-28T15:20:17.462402+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-73 noise=9999 txrate=13000
2026-01-28T15:20:20.467240+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-73 noise=9999 txrate=26000
2026-01-28T15:20:23.483213+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-73 noise=9999 txrate=26000
2026-01-28T15:20:26.482734+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-72 noise=9999 txrate=26000
2026-01-28T15:20:29.483578+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-74 noise=9999 txrate=19500
2026-01-28T15:20:32.500442+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-71 noise=9999 txrate=19500
2026-01-28T15:20:32.998838+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-71 noise=9999 txrate=19500
2026-01-28T15:20:33.513837+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-72 noise=9999 txrate=19500
2026-01-28T15:20:34.014220+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-75 noise=9999 txrate=19500
2026-01-28T15:20:37.028727+05:30 SH wpa_supplicant[1122]: wlo1: CTRL-EVENT-SIGNAL-CHANGE above=0 signal=-76 noise=9999 txrate=13000


---

## 4. Mini Troubleshooting 

1. Check service status  
   $ systemctl status ssh

2. Restart service  
   $ sudo systemctl restart ssh

3. Check logs  
   $ journalctl -u ssh

---
  
