# Lab 11-1: Enable Verbose System Boot
![](attachment/4590a885ed4e33af3e46f215c9ea60fd.png)

Remove quiet from GURB_CMDLINE_LINUX

```bash
sudo vi /etc/default/grub
```
Remove silent line
![](attachment/Pasted%20image%2020260103132729.png)

---
# Lab 11-2: Reset root User Password
![](attachment/Pasted%20image%2020260103133218.png)
Reboot the system then press e on the kernel you want to boot and append rd.break to the end of the linux line
![](attachment/Pasted%20image%2020260103133206.png)

```bash
chroot /sysroot
```

```bash
passwd
```
mount the / directory as read and write
```bash
mount -o remount,rw /
```
Change the root password
```bash 
passwd
```

```bash
touch /.autorelabel
```

---
