# Lab 10-1: Configure Access to RHEL 9 Repositories
![](attachment/5a2a27a7c43c6a9306dff3b874b42790.png)

Check to see if RHEL 9 image is attached and mounted
```bash
lsblk
ls -la /mnt
```

Create the definition file
```bash
sudo vi /etc/yum.repos.d/local2.repo
```
![](attachment/9f03a210b8de4dee2e210cebe8ee3ca2.png)
```bash
sudo dnf clean all
```
Then view the repolist
```bash
sudo dnf repolist
```
![](attachment/0c262aaccfbd00d94ad0ecc992ef33fc.png)

---
# Lab  10-2: Install and Manage Individual Packages
![](attachment/bf077cffe8d0d144011d3fae1e0441ab.png)

To List all installed and available packages 
```bash
dnf list --installed
dnf list --available 
```

Show which packages contains the /etc/group files
```bash
dnf provides /etc/group
```

Install the package httpd
```bash
sudo dnf install httpd
```

![](attachment/d5c9783a8090dda6df2d7984d8c9a22c.png)
Show info of httpd
```bash
dnf info httpd
```
List dependences
```bash
dnf deplist httpd
```

Remove it
```bash
sudo dnf remove httpd -y
```

---
# Lab 10-3: Install and Manage Package  Groups
List availble and installed package groups seperate

```bash
dnf group list available
dnf group list installed
```

Install package groups Security Tools and Scientific Support
```bash
sudo dnf group install "scientific support"
sudo dnf group install "security tools"
```

```bash
sudo tail /var/log/dnf.log
```
![](attachment/bfb4c31208899afe542cc8b4974b47fb.png)

Show info for scientific support package
```bash
dnf group info "Scientific Support"
```

Remove scientific support package
```bash
sudo dnf group remove "scientific support"
```
