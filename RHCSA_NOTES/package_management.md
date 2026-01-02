# Basic Package Management

**RPM (RPM Package Manager)** is Red Hat’s system for installing and managing software.  
Packages use the `.rpm` format and include files plus metadata (permissions, ownership, paths).  
Packages may depend on other packages or files; RPM tracks this in its package database.

Once installed, a package’s metadata is recorded so updates or changes can be managed cleanly.

The **rpm** command handles:
- Querying packages  
- Installing / upgrading / freshening  
- Overwriting existing installs  
- Removing packages  
- Extracting package contents  
- Verifying integrity and authenticity using GPG keys  

This chapter covers identifying package dependencies, checking the RPM database, verifying signatures, and using `rpm` to manage software.

---
# Package Overview
RHEL is a set of packages grouped together tto make an operating system built around the linux kernal.

---

# Packages and Packaging

A software package is a structured collection of files plus metadata that forms an application.  
Packages come in two types:

- **Binary packages (.rpm)** — ready to install; include executables, configs, libraries, docs, dependency data, and pre/post install scripts.
- **Source packages (.src)** — contain original source code that can be modified and rebuilt into binary packages.

Package metadata (version, install paths, checksums, file lists, attributes) is stored centrally so tools can manage installs, upgrades, dependencies, and removals.  
“Package intelligence” includes prerequisites, needed users/directories, and uninstall steps.

---
# Package Naming

Red Hat RPM packages use a consistent naming format with these parts:

**name-version-release.elX_arch.rpm**

Example:  
openssl-3.0.1-43.el9_0.x86_64.rpm → installed as openssl-3.0.1-43.el9_0.x86_64

Parts:
- **openssl** → package name  
- **3.0.1** → version  
- **43** → release/build  
- **el9_0** → RHEL version (may be absent)  
- **x86_64** → architecture (`noarch` = platform-independent, `src` = source package)  
- **.rpm** → installable package extension
---

# Package Dependency

A installable package may require one or more additional packages to work so they most be installed

---
# Package Database

- Installed package metadata is stored in **/var/lib/rpm**, known as the *RPM database*.  
- It tracks package names, versions, file ownership, permissions, timestamps, sizes, and dependency info.  
- RPM tools use this database to query, verify, install, upgrade, and remove packages.  
- When upgrading, RPM removes old metadata and adds new metadata.  
- RHEL 9 supports keeping multiple versions of the same package simultaneously.

---
# Package Management Tools
Redhat used rpm but rpm does not resolve dependencies

So now we use yum so thatt it resolves dependencies

A major enhancement to yum is dnf now that is the standard

---

# Package Management with rpm

Rpm handles these task: Querying, installing, updating, freshening, overwriting, removing, extracting, validating and verifying packages.

---
# Rpm command 

- `rpm` manages, queries, installs, verifies, upgrades, and removes RPM packages.
- Metadata comes from the RPM database in /var/lib/rpm.

## Key Query Options:
- `-q` : query package
- `-qa` : list all installed packages
- `-ql` : list files in a package
- `-qi` / `-qip` : show package info (installed / installable)
- `-qc` / `-qd` : show config/docs
- `-qf` : find which package a file belongs to
- `-qR` : list dependencies
- `--whatprovides` / `--whatrequires` : provider/requirement lookup

## Install / Remove / Verify:
- `-i` : install
- `-U` : upgrade or install if missing
- `-F` : freshen (upgrade only if installed)
- `-e` : erase/remove
- `--force` : override conflicts
- `-h` : show hash progress during install
- `-K` : check signature & integrity
- `-V` : verify installed package
- `--import` : import a GPG key
- `-v` / `-vv` : verbose output

---

# Querying Packages

To query all installed packages
```bash
rpm -qa
```

To query specific package
```bash
rpm -q perl
```

To list all files in a package
```bash
rpm -ql iproute
```

To list documentation files in a package
```bash
rpm -qd audit
```

To list only configuration files in a pckage
```bash
rpm -qc cups
```

To identify which package owns the specified file
```bash
rpm -qf /etc/passwd
```

To display information about the installed package 
```bash
rpm -qi setup
```

To list all file and package dependencies for a given package:
```bash
rpm -qR chrony
```

To a query an installable package for metadata information
```bash
rpm -qip /mnt/BaseOS/Packages/zsh-5.8-9.el9.x86_64.rpm
```

---

# Installing a Package

This command installs zsh
```bash
sudo rpm -ivh /mnt/BaseOs/Packages/zsh-5.8-9.x86_64.rpm
```

If this package requires the dependencies you will get an error message about failing to load dependencies must download additional packages 

---

# Upgrading a Package

Upgrading a package either upgrades installed package or simply installs a package. To upgrade a package use -U option

```bash
sudo rpm -Uvh /mnt/AppStream/Packages/sushi-3.38.1-2.el9.x8664.rpm
```

The command makes a backup of all affected configuration files after the upgrade process

---
# Freshening a Package

Freshening a package requires an older version to exist on a system

To freshen a sushi pacakge, use the -F option:

```bash
sudo rpm -Fvh /mnt/AppStream/Packages/sushi-3.38.1-2.el9.x86_64.rpm
```

---
# Overwriting a Package

Overwriting a pacakge replaces the files with the package of same version

Use --replacepkgs option


# Remove a package

To remove a package uninstalls a package and all associated files

To remove a package use -e option and -v for verbosity

Command performs a dependency check

---
# Extracing Files from an Installable Package

Files in an installable package can be extracted using the rpm2cpio command

Assuming you have lost the /etc/chrony.conf config file, you will need to determine what package this file comes from

```bash
rpm -qf /etc/chrony.conf
```


```bash
sudo rpm2cpio /mnt/BaseOS/Packages/chrony-4.2-1.el9.x86_64.rpm | cpio -imd
```

```bash
sudo find . -name chrony.conf 
```


---
# Validating Package Integrity and Credibility

## Purpose
- **Integrity**: Package is complete and unmodified
- **Credibility**: Package is from a trusted source (authentic)


## Integrity Check (Checksum)
- Uses **MD5**
- Verifies file was not corrupted

```bash
md5sum chrony-4.2-1.el9.x86_64.rpm

```

Import GPG Key
```bash
rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
 ```

Verify Package Signature
```bash
rpm -K chrony-4.2-1.el9.x86_64.rpm
 ```

---
# Viewing GPG Keys
## Purpose
- View imported **GPG public keys** used to verify RPM packages
## List Imported GPG Keys
```bash
rpm -qa gpg-pubkey*
```

---
# Verifying Package Attributes
## Purpose
- Verify **installed RPM files** against original attributes
- Detect unauthorized or accidental changes
## Verify Installed Package
```bash
rpm -V at
#Specific File
rpm -Vf /etc/sysconfig/atd
```
 Attribute Verification Codes (rpm -V)

| Code | Meaning                                 |
| ---: | --------------------------------------- |
|    S | File size changed                       |
|    M | Mode (permissions) or file type changed |
|    5 | MD5 checksum mismatch                   |
|    D | Device major/minor number changed       |
|    L | Symlink path changed                    |
|    U | Owner changed                           |
|    G | Group changed                           |
|    T | Timestamp changed                       |
|    P | Capabilities changed                    |
|    . | No change detected                      |
 File Type Codes

| Code | File Type          |
| ---: | ------------------ |
|    c | Configuration file |
|    d | Documentation file |
|    g | Ghost file         |
|    l | License file       |
|    r | Readme file        |

---
# Advance Package Management

Dnf command is superior to rpm tool since it performs automatic dependency checks and

---
# Advanced Package Management Concepts 

## Must Know
- **dnf** is the primary package manager in RHEL 9
- Handles dependencies automatically
- Manages single packages, multiple packages, and package groups

## rpm vs dnf
- `rpm` → low-level, no dependency resolution
- `dnf` → high-level, repository-aware (preferred)

## RHEL 9 Repositories
- **BaseOS** → core system packages
- **AppStream** → applications, languages, runtimes (streams)

## Exam Expectation
- Use **dnf** unless explicitly told to use `rpm`
- Understand where packages come from (BaseOS vs AppStream)

---
# Package Groups

## What
- **Package group** = related packages managed together

## Types
- **Environment groups** (system roles)
  - Server, Server w/ GUI, Minimal, Workstation, Virtualization Host
- **Package groups** (features)
  - Container Management, Security Tools, System Tools, Network Servers

## Exam Tip
- Environment = OS role
- Package group = feature set
- Managed with **dnf**

---
# BaseOs Repository
# BaseOS Repository – Exam Essentials (RHCSA)

- Contains **core RHEL 9 components**
- Includes:
  - Kernel
  - Kernel modules
  - Bootloader
  - Foundational system packages
- Required to run applications
- Packages provided as **RPMs**

---
# AppStream Repository

- Contains **applications and add-ons**
- Includes:
  - Web servers
  - Programming languages
  - Databases
  - Runtimes
- Provides:
  - Traditional **RPM packages**
  - **Modular (streamed) packages**
- Supports multiple versions and use cases

---
# BaseOS vs AppStream Segregation 

## Why Segregation Matters
- Separates **core OS** from **applications**
- Allows **independent updates**

## Benefits
- Core OS stability (kernel, system tools)
- Applications can be updated more frequently
- Avoids unintended upgrades

## Key Point
- BaseOS and AppStream can be updated **independently**
- Prevents application breakage due to OS updates

---
# dnf Repository

# dnf Repositories – Exam Essentials (RHCSA)

## Purpose
- Repos = software libraries for **dnf**  
- Used to install, update, query, remove packages  
- Prefer **trusted sources** (Red Hat, internal)

## Default RHEL 9 Repos
- **BaseOS** → core OS packages  
- **AppStream** → applications & streams

## Repo Configuration
- Defined in `.repo` files (`/etc/yum.repos.d/`)  
- Minimum required:
  - `[ID]` → unique
  - `name`
  - `baseurl`
- Example:
```ini
[BaseOS_RHEL_9]
name=RHEL 9 BaseOS
baseurl=file:///mnt/BaseOS
enabled=1
gpgcheck=0

```

## baseurl formats
Local: file:///path
FTP: ftp://host/path
HTTP/HTTPS: http(s)://host/path
Use double slashes (//) for network URLs

---
# Software Management with dnf

- Enterprise Linux packages = **RPM format**
- `rpm` → single-package management, limited
- `dnf` (or `yum`) → advanced tool for:
  - Single packages
  - Package groups
  - Automatic dependency resolution
- `dnf` uses **configuration files** to control behavior

---
## dnf Configuration File

## Key Points
- Main config: `/etc/dnf/dnf.conf`
- **Main section** → global dnf directives
- Custom repos → preferred in `/etc/yum.repos.d/`
## Important Directives

| Directive | Purpose |
|-----------|---------|
| best | Install/upgrade to latest version |
| clean_requirements_on_remove | Remove unused dependencies on package removal |
| debuglevel | Log debug info (0–10; default 2) |
| gpgcheck | Verify GPG signatures (default 1) |
| installonly_limit | Max concurrent installed packages (default 3) |
| keepcache | Keep package/header cache after install (default 0) |
| logdir | Log file directory (default `/var/log`) |
| obsoletes | Remove obsolete dependent packages (default 1) |

## Exam Tip
- Know **location of dnf.conf** and key directives  
- Use `man 5 dnf.conf` for full reference
---
# Configuring Access To Pre-built Repositories
1. Create Repo Definition File
	sudo vim /etc/yum.repos.d/local.repo
2. Add repositories:
[BaseOS_RHEL_9]
name=RHEL 9 BaseOS
baseurl=file:///mnt/BaseOS
enabled=1
gpgcheck=0

[AppStream_RHEL_9]
name=RHEL 9 AppStream
baseurl=file:///mnt/AppStream
enabled=1
gpgcheck=0

3. Confirm Repository Access
	dnf repolist

---
# Individual Package Management

You can use dnf to install query and remove packages

---
# Listing Available and Installed Packages
- `dnf install <package>` → installs package + dependencies; updates if older version exists; prompts for confirmation (use `-y` to skip)
- Local install: `sudo dnf install /mnt/AppStream/Packages/dcraw-*.rpm`
- Update specific package: `sudo dnf update autofs`
- Update all packages: `sudo dnf update`
- Key points: resolves dependencies automatically, shows download size, disk usage, and installed packages

---
# Viewing Package Information with dnf – RHCSA

- Use `dnf info <package>` to display:
  - Name, architecture, version, release, size
  - Installation status
  - Repository source
  - Short & long description, license
- Example:
```bash
dnf info autofs
```

---
# Removing a Package

- `dnf remove <package>` → uninstalls package + dependencies, removes files and directories
- Prompts for confirmation by default; use `-y` to skip
- Example:
```bash
sudo dnf remove keybinder3
```

---
# Determining Provider and Searching Package Metadata

- Find which package provides a file:
```bash
dnf provides /etc/passwd
# or
dnf whatprovides /etc/passwd
```

---
# Package Group Management

## Group Types
- **Environment groups** → full system roles (e.g., Server with GUI)
- **Package groups** → related packages for a specific functio
## List Package Groups
- List all installed & available groups:
```bash
dnf group list
dnf group list --count     
dnf group list --hidden    
dnf group list --installed
dnf group list --available
```

```bash
dnf group info "Base"
dnf group info -v "Base"
```

---
# Installing and Updating Package Groups
- Install or update a package group:
```bash
sudo dnf group install "Smart Card Support"
```

- Update an installed package group:

```bash
sudo dnf group update "Smart Card Support"
```

---
# Removing Package Groups

- Removing a package group:
```bash
sudo dnf -y groupremove 'smart card support'
```

---
