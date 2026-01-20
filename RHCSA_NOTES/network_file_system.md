# Chapter 16: Network File System (NFS)

This chapter covers the concepts and configuration of **Network File System (NFS)** and **AutoFS** on RHEL systems.

## Topics Covered
- Overview of the NFS service and its key components  
- Benefits of NFS and supported versions  
- Exporting directories (shares) on an NFS server  
- Mounting NFS shares on a client using the standard `mount` command  
- Understanding the **AutoFS** service, its benefits, and how it works  
- AutoFS configuration maps  
- Mounting NFS shares using AutoFS  
- Sharing and mounting user home directories with NFS and AutoFS  
## Overview
NFS allows remote file systems to be mounted and accessed on RHEL clients just like local file systems. These mounts can be handled manually using standard mount tools or automatically using **AutoFS**.

AutoFS dynamically mounts file systems when they are accessed and unmounts them after inactivity, reducing resource usage and administrative overhead. In real-world environments, NFS is commonly paired with AutoFS to provide scalable, efficient file sharing, including centralized user home directories.

This chapter walks through both server-side and client-side configurations and demonstrates practical use cases for NFS and AutoFS.

---
# Network File System (NFS)

**NFS** is a protocol for sharing files over a network using a **client/server architecture**:

- **NFS Server:** Provides shares (files, directories, or entire file systems) for remote access. The process of making shares available is called **exporting**.  
- **NFS Client:** Accesses exported shares from the server as if they were local. The process of making them available locally is called **mounting**.  

A system can act as both **server and client** simultaneously.  
- Exported shares include the full directory tree beneath them.  
- A subdirectory or parent of an exported share cannot be re-exported within the same file system.  
- Mounted shares cannot be re-exported.  
- Each exported share is mounted to a **single mount point** on the client.

---
# NFS Versions

RHEL 9 supports **NFSv3, NFSv4.0, NFSv4.1, and NFSv4.2** (default: 4.2):

- **NFSv3:**  
  - Supports TCP & UDP  
  - Asynchronous writes  
  - 64-bit file sizes (files >2GB)  

- **NFSv4.x (4.0, 4.1, 4.2):**  
  - Built on NFSv3 features  
  - Internet-friendly, firewall-transit capable  
  - Enhanced security with optional encrypted transfers  
  - Uses usernames/groups instead of UIDs/GIDs  
  - Improved scalability, cross-platform support, and crash handling  
  - Protocol defaults:  
    - 4.0 & 4.1 → TCP (UDP optional)  
    - 4.2 → TCP only

---
# Export Share on NFS Server

1. Install the NFS software called nfs-utils:
```bash
sudo dnf -y install nfs-utils
```
2. Create /common directory to be exported as a share:
```bash
sudo mkdir /common
```
3. Add permissions to common
```bash
sudo chmod 777 /common
```
4. Add the NFS service persistently to the Linux firewall to allow NFS traffic
```bash
sudo firewall-cmd --permanent -add-service nfs
```
5. Start the NFS service and enable it to autostart
```bash
sudo systemctl --now enable nfs-server
```
6. Verify the operational status of the NFS service:
```bash
sudo systemctl status nfs-server
```
7. Open the /etc/exports file in a text editor and add an entry for /common to export it to server10 with read/write
```bash
sudo vim /etc/exports
#In file
/common server10(rw)
```
8. Export the entry defined in /etc/exports file. The -a option exports all entries
```bash
sudo exportfs -av
```
9. Unexport the entry 
```bash
sudo exportfs -u server10:/common
```

---
# Mount Share on FS Client
`
![](../RHCSA_Labs/attachment/Pasted%20image%2020260116165712.png)
```bash
sudo dnf -y install nfs-utils
```
2. Create /local mount point:
```bash
sudo mkdir /local
```
3. Mount the share manually using the mount command:
```bash
sudo mount server20:/common /local
```
4. Confirm the mount using either the mount or the df command:
```bash
mount | grep local
```
5. Open hte /etc/fstab file and append the following entry for persistence:
```bash
sudo -E vim /etc/fstab
```
![](../RHCSA_Labs/attachment/Pasted%20image%2020260119152452.png)

6. Unmount the share manually using the unmount command and remount it via the fstab file
```bash
sudo umount /local
sudo mount -a
```
8. Create a file called nfsfile under /local and verify:
```bash
touch /local nfsfile
```
9. Confirm the file creation on the NFS server 
```bash
ls -l /common
```
![](../RHCSA_Labs/attachment/Pasted%20image%2020260119153456.png)

---
# Auto File System (AutoFS)

- **AutoFS** is a client-side service that automatically mounts and unmounts NFS shares **on demand**.
- Unlike manual mounting, AutoFS mounts a share only when activity is detected in its mount point (e.g., `ls`, `cd`).
- If the share remains unused for a configured time, AutoFS automatically unmounts it.
- AutoFS-managed mounts **must not** be mounted manually or defined in `/etc/fstab`.
- This approach reduces unnecessary kernel resource usage and slightly improves system performance.
- AutoFS supports mounting both during runtime and across system reboots through its configuration files.

---
# Benefits of Using AutoFS

- **Uses AutoFS maps, not `/etc/fstab`:** NFS shares are defined in map files located in `/etc` or `/etc/auto.master.d`, keeping mounts separate from static filesystem entries.
- **No root privileges required:** Users can trigger mounts without needing root access, unlike manual or fstab-based mounts.
- **Prevents system hangs:** If an NFS server is down or unreachable, AutoFS avoids client hangs that can occur with fstab mounts.
- **Automatic unmounting:** Shares are unmounted automatically after inactivity (5 minutes by default), conserving system resources.
- **Dynamic features:** Supports wildcard characters and environment variables, offering flexibility not available with fstab-based mounting.

---
## AutoFS Configuration File

The configuration file for the AutoFS service is /etc/autofs.conf, which AutoFS consults at service startup. Some key directives from this file are shown below along with preset values:
master_map_name = auto.master

timeout = 300

negative_timeout = 60

mount_nfs_default_protocol = 4

logging = none

There are additional directives available in this file and more can be added to modify the default behavior of the AutoFS service. Table 16-1 describes the above directives.

|                            |                                                                                                                           |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Directive                  | Description                                                                                                               |
| master_map_name            | Defines the name of the master map. The default is auto.master located in the /etc directory.                             |
| timeout                    | Specifies, in seconds, the maximum idle time after which a share is automatically unmounted. The default is five minutes. |
| negative_timeout           | Expresses, in seconds, a timeout value for failed mount attempts. The default is one minute.                              |
| mount_nfs_default_protocol | Sets the default NFS version to be used to mount shares.                                                                  |
| logging                    | Configures a logging level. Options are none, verbose, and debug. The default is none (disabled).                         |
The directives in the autofs.conf file are normally left to their default values, but you can alter them if required.

---

## Direct Map
- Mounts shares automatically on unrelated mount points.
- Features:
- Shares are always visible.
- Local and direct shares can coexist under one parent.
- Accessing a directory with many direct mounts mounts all shares.
- Each entry adds a separate entry to `/etc/mtab`.
- Example: `/etc/auto.master.d/auto.direct` referenced from master map.

## Indirect Map
- Mounts shares under a common parent directory.
- Features:
- Shares are visible only after access.
- Local and indirect shares cannot coexist under the same parent.
- Each map adds only one entry to `/etc/mtab`.
- Example: `/etc/auto.misc` referenced from master map under `/misc`.
- Commonly used for removable media (CD/DVD/USB).

## Direct vs Indirect Map
- **Direct map:** shares are immediately visible; suitable for static mount points.
- **Indirect map:** shares mount on demand; efficient for automounting NFS shares.
- Indirect maps are generally preferred but choice depends on environment requirements.

---
# Access NFS Share Using Direct Map

![](../RHCSA_Labs/attachment/Pasted%20image%2020260119161624.png)
1. Install the AutoFs software package called autofs:
```bash
sudo dnf install -y autofs
```
2. Create a mount point /autodir using the mkdir command:
```bashj
sudo mkdir /autodir
```
3. Edit the /etc/auto.master file and add the following entry at the beginning off the file. This entry will point he AutoFs service to the auto.dir file for additional information.
```bash
/-  /etc/auto.master.d/auto.dir
```
4. Create /etc/auto.master.d/auto.dir file and add the mount point NFS server, and share information to it:
```bash
/autodir server20:/common
```
5. Start the AutoFS service and set it to autostart at system reboot
```bash
sudo systemctl enable --now autofs
```
6. Verif the ooperational status fo the AutoFs Service
```bash
sudo systemctl status autofs -l --no-pager
```
7. Run the ls command on the mount point /autodir and then issue the mount command to verify the mount
```bash
ls /autodir
```

---
# Access NFS Share Using Indirect Map 

![](../RHCSA_Labs/attachment/Pasted%20image%2020260119164030.png)
1. Install the autofs software package if not there:
```bash
sudo dnf -y install autofs
```

2. Confirm the entry for the indirect map /misc in the /etc/auto.master file exists:
```bash
grep ^/misc /etc/auto.master
```
3. Edit the /etc/auto.misc file and add the mount point, NFS server, and share information to it:
```bash
autoindir server20:/common
```
4. Restart the autofs service
```bash
sudo syystemctl enable --now autofs
```
5. Verify the operational status of the AutoFS service
```bash
sudo systemctl status autofs -l --no-pager
```
6. Run the ls command on the mount point /misc/autoindir and then grep
```bash
ls /misc/autoindir
mount | egrep 'auto.misc|autoindir'   
```

---
# Automounting User Home Directories

AutoFS can automount user home directories using **indirect map substitutions**:

- `*` → replaces references to specific mount points (usernames).  
- `&` → replaces references to NFS servers and shared subdirectories.

For user home directories under `/home` on one or more NFS servers, AutoFS mounts **only the specific user’s home directory** when they log in, rather than the entire `/home`.  

Example indirect map entry (`/etc/auto.master.d/auto.home`):

- `auto.home` uses `*` and `&` to dynamically reference usernames and NFS servers.  

**Benefits:**
- No need to update AutoFS files when adding/removing NFS servers or home directories.
- Supports multiple NFS servers simultaneously.
- If only one NFS server exists, the first `&` can be replaced with the server name.

---
# Automount User Home Directories Using Indirect Map
![](../RHCSA_Labs/attachment/Pasted%20image%2020260119165154.png)
1. Create a user account called user30 with UID 3000 and assign password
```bash
sudo useradd -u 3000 user30
echo password1 | sudo passwd --stdin user30
```
2. Edit the etc/exports file and add an entry for /home
```bash
/home 192.168.22.56(rw)
```
3. Export all the shares listed in the /etc/exports file:
```bash
sudo exportfs -avr
```
4. On server 10 now install autofs if not there:

5. Create a user account called user30 with UID 3000
```bash
sudo useradd -u 3000 -b /nfshome -M user30
echo password1 | sudo passwd --stdmin user30
```
6. Create the umbrella mount point /nfshome to automount the user's home directory:
```bash 
sudo mkdir /nfshome
```
7. Edit the /etc/auto.master file and add the mount point and indrect map location to it 
```bash
/nfshome /etc/auto.master.d/auto.home
```
8. Create the /etc/auto.master.d/auto.home file and add the following information to it:
```bash
*  -rw 192.168.122.2:/home/&
```
9. Start AutoFs service to autostart
10. Verify the operation status of the AutoFS service
```bash
sudo systemctl status autofs -l --no-pager
```
11. Login in as user30 and run pwd 
```bash
su - user30
pwd
```

---
