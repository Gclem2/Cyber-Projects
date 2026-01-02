# Linux Boot Process
1. **Firmware** (BIOS/UEFI)
2. **Bootloader** (GRUB2)
3. **Kernel**
4. **Init/Systemd** → target reached

## GRUB2 Key Facts
- Allows:
  - Booting into **different targets**
  - Passing **kernel parameters**
  - **Resetting root password**
- Can interrupt boot to modify kernel options temporarily
## Boot Targets
- System boots to a **default target**
- Can boot into a **non-default target** via GRUB2
## Kernel Essentials
- Core of the OS
- Controls:
  - Hardware
  - Processes & scheduling
  - Security & access control
- Modular (supports kernel modules)
- Installed/upgraded via RPM packages

---
# Firmware Phase (BIOS & UEFI) 

## Purpose
- First boot stage
- Initializes hardware and loads bootloader

## BIOS
- Legacy, 16-bit
- Runs **POST**
- Reads **MBR** (512 bytes):
  - 446 bootloader
  - 64 partition table
  - 2 signature
- Boot order configurable
- Loads bootloader → hands off control

## UEFI
- Modern replacement for BIOS
- 32/64-bit, faster
- Built-in boot manager
- Reads filesystems directly
- Supports multiple bootloaders

## Exam Tip
- Firmware loads **bootloader**, not kernel
- Know BIOS vs UEFI difference
 ---
# Bootloader Phase 

## Key Facts
- Runs after firmware detects boot device
- RHEL uses **GRUB2**
- Works with **BIOS and UEFI**

## Responsibilities
- Locate Linux kernel in `/boot`
- Decompress and load kernel into memory
- Read boot configuration
- Transfer control to kernel

## Config Locations
- **BIOS systems**:
  - `/boot/grub2/grub.cfg`
- **UEFI systems**:
  - EFI partition: `/boot/efi`
  - GRUB binary: `/boot/efi/EFI/redhat/grub.efi`

## Exam Tip
- GRUB2 = kernel selection, boot options, recovery access

 ---
# Kernel Phase
## Role
- Core of the OS
- Controls hardware and system services
## Boot Actions
- Receives control from **GRUB2**
- Loads and mounts **initrd** from `/boot`
- Mounts initrd as temporary root at `/sysroot` (read-only)
- Loads required modules and drivers
- Switches to real root filesystem `/` (read/write)
- Unmounts initrd
## Handoff
- Starts **systemd (PID 1)**
- Passes control to initialization phase

## Exam Tip
- Kernel starts **systemd**
- initrd enables disk and filesystem access during boot
---
# Initiation Phase
Final boot stage. systemd (PID 1) takes control from the kernel, starts all enabled services, and brings the system to the configured boot target. Boot completes when target services are running and a login prompt is available.

---
# The GRUB2 Bootloader
- After firmware, **GRUB2** displays a menu of available kernels and autoboots the default after ~5 seconds.
- Press any key to interrupt autoboot.

### GRUB2 Controls
- **Up/Down arrows**: Select a kernel
- **e**: Temporarily edit kernel boot parameters (from `/boot/grub2/grub.cfg`)
- **c**: Enter the `grub>` command prompt

### Common Exam Tasks
- Append to the `linux` line to change boot behavior:
  - `3` → Multi-user (text) mode
  - `rescue` → Rescue mode
  - `emergency` → Emergency mode
- Press **Ctrl+x** to boot with changes
- Changes are **temporary** and do **not** modify configuration files

---
# Reset the root User Password
1. **Reboot** server1 and interrupt GRUB2.
   - Select the default/rescue kernel
   - Press **e** to edit

2. Find the line starting with `linux` and append:
		rd.break

3. Press **Ctrl+x** to boot.
- System stops in an early **initramfs shell**
- Root filesystem is mounted **read-only** at `/sysroot`

4. Mount root filesystem as `/`:
```bash
chroot /sysroot
```

```bash
mount -o remount,rw /
```

```bash
passwd
```

```bash
touch /.autorelabel
```

```bash
exit
exit
```

---
# The Linux Kernel
- The **kernel** is the core of Linux.
- Manages **hardware**, **security**, **processes**, **services**, and **system access**.
- Built from **modules** (device drivers + subsystems):
  - CPU, memory, storage, peripherals
  - Filesystems, networking, virtualization
### Kernel Modules
- **Static modules**: always built-in, essential
- **Dynamic modules**: loaded as needed → better performance, stability
### RHEL 9 Kernel
- Default kernel: **5.14.0**
- Architecture check:
  ```bash
  uname -m
```
---
## Kernel Packages (RHEL 9)

Kernel software is delivered as **RPM packages**, just like other RHEL components. A minimum set of core packages is required for system operation, with optional add-ons for extended functionality.

### Core & Add-On Kernel Packages

| Package                  | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| **kernel**               | Meta package; ensures required kernel packages are installed |
| **kernel-core**          | Minimal kernel and core modules                              |
| **kernel-modules**       | Modules for common hardware                                  |
| **kernel-modules-extra** | Modules for less common hardware                             |
| **kernel-devel**         | Files needed to build kernel modules                         |
| **kernel-headers**       | Interfaces between kernel and userspace programs             |
| **kernel-tools**         | Utilities for kernel management                              |
| **kernel-tools-libs**    | Libraries required by kernel tools                           |

### Notes
- Kernel **source packages** are available for customization and recompilation.
- A standard RHEL installation loads **multiple kernel packages** by default.
- Installed kernel packages can be viewed with:
  ```bash
  rpm -qa | grep kernel
```
---
# Installing the Kernel

See Lab 11-3 for indepth installation