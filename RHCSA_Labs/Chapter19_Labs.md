# Labs 19-1: Add service to Firewall

![](attachment/Pasted%20image%2020260123155243.png)
1. Add the HTTPS Service persistently
```bash
sudo firewall-cmd --permanent --add-serverice=https
```
2. Reload firewall
```bash
sudo firewall-cmd --reload
```
3. Verify with firewall-cmd
```bash
sudo firewall-cmd --list-services
```
4. Verify in the XML file
```bash
sudo cat /etc/firewalld/zones/public.xml
```
![](attachment/Pasted%20image%2020260123155658.png)

---
# Lab 19-2: Add Port Range to Firewall

## Objective
Add permanent UDP port range 8000-8005 to trusted zone on server30

## Steps

### 1. Add Port Range (Permanent)
```bash
# As user1 with sudo on server30
sudo firewall-cmd --permanent --zone=trusted --add-port=8000-8005/udp
```

### 2. Reload Firewall (Activate Change)
```bash
sudo firewall-cmd --reload
```

### 3. Verify with firewall-cmd
```bash
# List ports in trusted zone
firewall-cmd --zone=trusted --list-ports

# Or list all settings for trusted zone
firewall-cmd --zone=trusted --list-all
```
Expected output: `8000-8005/udp`

### 4. Verify in XML File
```bash
# Check trusted zone configuration file
sudo cat /etc/firewalld/zones/trusted.xml
```

---
# Chapter 20: Security Enhanced Linux

## Overview
SELinux is a kernel-level mandatory access control (MAC) mechanism that provides security beyond traditional DAC (user/group/permissions)

## Purpose
- Control **who** can access **what** on the system
- Limit damage from unauthorized user or program access
- Enforce security policies beyond standard file permissions

## Traditional Security vs SELinux
**Traditional (DAC - Discretionary Access Control)**:
- File/directory permissions (rwx)
- User and group ownership
- Shadow passwords and password aging

**SELinux (MAC - Mandatory Access Control)**:
- Additional layer on top of DAC
- Context-based access control
- Policy enforcement at kernel level

## RHCSA Objectives Covered
- **55**: Set enforcing and permissive modes
- **56**: List and identify file and process contexts
- **57**: Restore default file contexts
- **58**: Manage SELinux port labels
- **59**: Use Boolean settings to modify SELinux
- **60**: Diagnose and address SELinux policy violations

## Topics Covered
1. SELinux terminology and concepts
2. Contexts for users, processes, files, and ports
3. Copy/move/archive files with SELinux context
4. Domain transitioning
5. SELinux Booleans
6. Query and manage SELinux (tools)
7. Modify contexts for files and ports
8. Add SELinux rules to policy database
9. View and analyze SELinux alerts

---
