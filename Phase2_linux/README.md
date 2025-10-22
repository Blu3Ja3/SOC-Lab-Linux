Linux User Management and Log Analysis

In this exercise, I practiced adding and removing users in Linux, then reviewed the activity in the system logs — just like a SOC analyst investigating account changes.

## Step 1: Create New Users

```bash
sudo adduser weston
sudo adduser lilly
sudo adduser ava
sudo adduser sage
```
Each command creates a new user account.

### Simulate Login Activity

In a new terminal, I attempted to log in as weston using a wrong password first.
This helps me locate failed login attempts later in the logs.

### Investigate Authentication Logs

View authentication-related events:
```bash
cat /var/log/auth.log
```

This log shows:

Successful logins
Failed password attempts
Sudo usage
Account creation/deletion activity


<img width="1216" height="279" alt="image" src="https://github.com/user-attachments/assets/cf812c28-a014-4d72-8d3e-de3d8ef3712f" />




Since the log can get long, I used **grep** to find failed login attempts quickly:
```bash
grep "Failed" /var/log/auth.log
```


<img width="1002" height="154" alt="image" src="https://github.com/user-attachments/assets/1f1e07e2-cbd9-4766-9da6-a04218b66f46" />


Key takeaway: Always search for the right keywords when investigating logs

There are 5 failed log ins. This is good to be aware of these logs in the real world.


### Delete the Test Users

After testing, I removed the accounts:
```bash
sudo deluser weston
sudo deluser lilly
sudo deluser ava
sudo deluser sage
```
To confirm, I reviewed the logs again:
```bash
grep "removed" /var/log/auth.log
```
This verified that each deletion was logged successfully.
<img width="1210" height="132" alt="image" src="https://github.com/user-attachments/assets/b1e91525-833a-4be1-9374-3a9442984459" />


### Analyst Takeaway

Even simple user management actions generate valuable forensic data.
A SOC analyst can correlate:

Repeated failed logins (possible brute force)
New account creation (potential persistence)
Account deletion (covering tracks)
Monitoring /var/log/auth.log helps detect and respond to these events early.

#cybersecurity #blueteam #linux #socanalyst #homelab #loganalysis
