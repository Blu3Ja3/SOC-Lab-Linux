# SOC-Lab-Linux
This is my journey of working in Linux and preparing for a SOC role

# 🐧 Day 1 — Linux Basics & Log Analysis (SOC Analyst Practice)

**Author:** Jenna  
**Project:** SOC Analyst Blue Team Lab  
**Phase:** 2 — Blue Team Core Skills  
**Date:** 2025-10-22  

---

## 🎯 Objective
Strengthen foundational Linux and SOC analyst skills by:
- Exploring critical log files (`auth.log`, `syslog`, `kern.log`, `dmesg`)
- Understanding how everyday system actions appear in logs
- Practicing real-time log monitoring with `tail -f`
- Researching log entries to connect system behavior to security meaning

---

## 🧭 System Reconnaissance

Before diving into logs, I ran basic recon commands to re-familiarize myself with Linux:

```bash
whoami                     # current user
uname -a                   # system info
uptime                     # system load and uptime
df -h                      # disk space
du -sh /var/log            # disk use in log folder
top                        # processes
ps aux | less              # all running processes
```
These give me a quick snapshot of the system’s state — perfect for baselining before analysis.

### Exploring Logs

```bash
cd /var/log
ls
```
I began examining the most important log files for SOC analysts:
```bash
cat auth.log      # login attempts, sudo usage, SSH logins
cat syslog        # system-wide events
cat kern.log      # kernel-level activity
cat dmesg         # device and boot messages
```
### auth.log
While reviewing auth.log, I saw this line:

add 'jenna' to shadow group 'adm'

I wasn’t sure what “shadow group” meant — research showed it indicates that Jenna was added to the adm group, which has access to system logs.

SOC Insight:
In a real environment, that’s an elevation of privilege event worth verifying.

Command used to trace it:
```bash
grep "add" "jenna" /var/log/auth.log
```

This revealed who performed the change and when — confirming it was expected.

### Watching logs in real time
To see how my actions appear live:

Terminal 1 (monitoring):
```bash
sudo tail -f /var/log/auth.log
```

Terminal 2 (activity):
```bash
sudo ls /root
```

I intentionally entered a wrong password first, then the correct one.
Back in Terminal 1, I saw:

authentication failure for user jenna

Direct proof that my failed login attempt was logged — great real-time visibility.

### syslog
Running:
```bash
cat syslog
```

I noticed:

negative trust anchors

That sounded suspicious, so I researched it.

A trust anchor is a known-good cryptographic key used in DNSSEC.
The message “negative trust anchors” means the system skips DNSSEC validation for internal/private zones — normal behavior!

A suspicious variation would be:

systemd-resolved: DNSSEC validation failed for github.com

That could indicate DNS spoofing or tampering attempts.

### kern.log
```bash
cat kern.log
```
I found:

long readout interval, skipping watchdog check

“Watchdog” is a hardware timer ensuring the system remains responsive.
This log means the system skipped one check due to high CPU load — not an error.

Still, keep an eye on watchdog timeouts or kernel panics — they can signal instability or hardware failure.

### dmesg
```
cat dmesg
```
Nothing suspicious appeared this time, but I learned that dmesg captures:

Boot logs

Kernel driver messages

Device insertions (like USBs)

💡 Next Lab Idea:
I’ll insert a USB drive and see the detection event in dmesg — perfect for forensic training.

### Reflection
Today I revisited Linux fundamentals through a security lens.
Each log told a story — from authentication events to kernel watchdogs — and I got hands-on practice connecting commands to real log output.

Learning something new every day!

#cybersecurity #blueteam #socanalyst #linux #homelab #infosec #loganalysis
