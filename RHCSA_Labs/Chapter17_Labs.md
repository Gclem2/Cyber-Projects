# Lab 17-1: Configre Chrony

![](attachment/Pasted%20image%2020260121170855.png)
1. Install chrony
```bash
sudo dnf -y install chrony
```

2. Enable Chrony at boot
```bash
sudo systemctl enable --now chronyd
```
3. Edit chrony configuration file comment out lines containing pool or server.
```bash
sudo vim /etc/chrony.conf
server 127.127.1.0 # Add to the end of the file
```
4. Start the chrony service
```bash
sudo systemctl restart start chrony
```
5. View the sources
```bash
chronyc sources
```
![](attachment/Pasted%20image%2020260121171705.png)

---
# Lab 17-2 Modify System Date and Time

![](attachment/Pasted%20image%2020260121171833.png)

1. Check the current time with date
```bash
date
timedatectl
```
![](attachment/Pasted%20image%2020260121171957.png)
2. Change the system date to a future date verify 
```bash
sudo timedatectl set-ntp false
sudo timedatectl set-time 2030-01-01
timedatectl #Verify
```
![](attachment/Pasted%20image%2020260121172205.png)

3. Change the system time to one hour ahead verify
```bash
sudo date -s "+1 hour"
date
```
![](attachment/Pasted%20image%2020260121172325.png)
4. Reset date and time using NTP verify
```bash
sudo timedatectl set-ntp true
timedatectl
```

---
