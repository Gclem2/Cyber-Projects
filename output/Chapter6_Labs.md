![](attachment/5aff984a66a909ecba961b0ac53afc3f.png)
# Lab 6-1 Create User and Configure Password Aging  
You can use groupadd and man -h if you need to remember the flags
``` bash
groupadd -g 6000 lnxgrp
```
``` bash
useradd -g 6000 -u 5000 user5000
```
``` bash
passwd user5000
```
``` bash
chage -m 4 -M 30 -W 10 user5000
```
``` bash
chage -E 2023-12-20 user5000
```
``` bash
chage -l user5000
```
![](attachment/055a193e6e06efcda5b44a741852e2ed.PNG)

# Lab 6-2 Lock and Unlock user
![](attachment/c49cf3d80ea554061ca238ff9d5fed6a.PNG)
``` bash
passwd -l user5000
```
``` bash
sudo cat /etc/shadow
```

![](attachment/d2b5ca33bd970f64a6301fa75ae2eb22.png)
The ! infront of the hash indicates the account is locked

![](attachment/9faacf564d1de7eb6b16852af2c1d34a.PNG)

The authentication failure is because the account was properly locked

``` bash
usermod -U user5000
```

``` bash
grep user5000 /etc/shadow
```

![](attachment/4e517522875a5fbdb8b7aa3326dc3e9d.PNG)

# Lab 6-3 Modify group
``` bash
groupmod -g 7000 lnxgrp
```
``` bash
usermod -aG lnxgrp user1000
usermod -aG lnxgrp user2000
```
``` bash
groupmod -n  dbagrp lnxgrp
```
``` bash
grep dbagrp /etc/group
```
![](attachment/69a6b4abb4660bc2eb1a831cf6bf3125.PNG)

# Lab 6-4 Configuring Sudo Acess
![](attachment/6560392bfacb3356da1fd519ddc43bf2.PNG)
```bash 
visudo
```
Put this text in the visudo file
![](attachment/b910156d2ccf0984979215dc73a3e8a8.PNG)
You can see after running the vgs command in sudo there is no password prompt
![](attachment/caed09e2fe84780a3e717f65b809f15a.PNG)

# Lab 6-5 Modifying Owing User and Group 

![](attachment/0890444223ab2b6f79c76ab098963046.png)

``` bash 
touch /tmp/f6 
mkdir /tmp/d6 
```

``` bash 
sudo useradd user90 
sudo chown user90 /tmp/f6
sudo chgrp dbagrp /tmp/f6 
sudo groupadd g1 
sudo chown -R user90:g1 /tmp/d6
```




