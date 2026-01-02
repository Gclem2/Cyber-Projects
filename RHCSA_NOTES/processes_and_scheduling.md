# Chapter 08 — Linux Processes and Job Scheduling

## Overview

This chapter explains how Linux handles **processes** and **job scheduling**, covering how to view, manage, and control processes, as well as schedule tasks for future execution.

---

## Key Topics

- Identify and display **system** and **user processes**  
- View **process states** and **priorities**  
- Adjust **niceness** and **priority** values  
- Use **signals** to control processes (terminate, restart, etc.)  
- Understand **job scheduling** concepts  
- Manage scheduling **permissions**  
- Schedule one-time tasks with **`at`**  
- Schedule recurring tasks with **`cron`**  
- Understand **crontab syntax** and management  

---

## RHCSA Objectives

1. Identify CPU/memory intensive processes, adjust priority with `renice`, and kill processes  
2. Adjust process scheduling  
3. Schedule tasks using `at` and `cron`  

---

## Summary

A **process** is any running program, command, or application managed by the **kernel**. Each process has a unique **PID** (Process ID) and can exist in various **states** during its lifecycle. Processes can run in the terminal or as background **services**, and their priority can be modified at launch or while running.

Processes respond to **signals**, which control their behavior—such as stopping, restarting, or terminating them safely or forcefully.

**Job scheduling** lets users automate commands for future execution.  
- **`at`** handles one-time jobs.  
- **`cron`** handles recurring jobs via **crontab** entries.  
Only authorized users can schedule tasks, and all job activity is logged for accountability.
---
## Processes and Priorities

Process's are created in memory when a program, application or command is initiated. Each process has a parent process that spawns it and each process has a PID to identify it to the kernel. When a process completes its lifespan the event is reported to the parent then resources are freed. Many processes are spawned at boot but some sit in memory and wait for an event to trigger to use them. Background processes are called **daemons**

---
## Process States
Processes can be in these states
Running: The process is being executed by the system CPU.
Sleeping: The process is waiting for input from a user or another process.
Waiting: The process has received the input it was waiting for and is now ready to run as soon as it turn comes.
Stopped: the process is currently halted and will not run even when its turn comes unless a signal is sent to change its behavior. (Signals are explained later in this chapter.)
Zombie: The process is dead. A zombie process exist in the process table alongside other process entries, but it takes up no resources. Its entry is retained until its parent process permits it to die. A zombie process is also called a defunct process.

![[Pasted image 20251106232955.png]]

---
## Viewing and Monitoring Processes with ps
# Viewing and Monitoring Processes with ps

A system may have hundreds or thousands of processes running concurrently depending on the purpose of the system. These processes may be viewed and monitored using various native tools such as **ps** (process status) and **top** (table of processes). The **ps** command offers plentiful switches that influence its output, whereas **top** is used for real-time viewing and monitoring of processes and system resources.

Without any options or arguments, **ps** lists processes specific to the terminal where this command is issued:

The output returns basic information about processes in four columns. These processes are tied to the current terminal window. It shows the **PID**, the terminal (**TTY**) the process spawned in, the cumulative CPU time (**TIME**) the system has given to the process, and the name of the actual command or program (**CMD**) being executed.


### Common ps Options

Some common options used with **ps** to generate detailed reports include:

- **-e** → show every process  
- **-f** → full-format listing  
- **-F** → extra full-format  
- **-l** → long format  

A combination of **-eFl** (`ps -eFl`) produces a very thorough process report, though that much detail may not be needed in most cases.  

Other useful options include:
- **--forest** → shows process hierarchy as a tree  
- **-x** → includes daemon processes (not attached to a terminal)  

Use the manual page (`man ps`) for more options and details.

### ps -ef Output Columns

When **ps** is executed with **-e** and **-f**, it displays eight columns of information for all running processes.

| Column    | Description                                                        |
| --------- | ------------------------------------------------------------------ |
| **UID**   | User ID or name of the process owner                               |
| **PID**   | Process ID of the process                                          |
| **PPID**  | Process ID of the parent process                                   |
| **C**     | CPU utilization for the process                                    |
| **STIME** | Process start date or time                                         |
| **TTY**   | Controlling terminal; “console” for system console, “?” for daemon |
| **TIME**  | Aggregated execution time                                          |
| **CMD**   | Command or program name                                            |

*Table 8-1: ps Command Output Description*

### Process Hierarchy and Details

The **ps** output reveals multiple **daemon processes** running in the background. These are not associated with any terminal, indicated by a **?** in the **TTY** column.

- Smaller **PID** numbers indicate earlier system start times.  
- **PID 0** starts first at system boot, followed by **PID 1**, and so on.  
- Each process has an associated **PPID** (parent process ID).  
- The **UID** column shows the owner of the process.  

Information for each running process is stored in the **/proc** filesystem, which **ps** and other tools use to gather data.

### Customizing ps Output

You can customize **ps** to display specific columns using the **-o** option.  
For example, to show `CMD`, `PID`, `PPID`, and `USER` in that order:

---


## Viewing and Monitoring Processes with top

The other popular tool for viewing process information is the **top** command. This command displays statistics in **real time** and continuously, helping identify performance issues on the system.

Press **q** or **Ctrl+c** to quit.

---

### Structure of top Output

The **top** output has two main sections:

1. **Summary portion** – first five lines  
2. **Tasks portion** – detailed process list below

---

#### Summary Portion (Lines 1–5)

- **Line 1:** Shows system uptime, number of logged-in users, and system load averages (1, 5, and 15 minutes).  
- **Line 2:** Displays total tasks (processes) and counts of those running, sleeping, stopped, or zombie.  
- **Line 3:** Shows processor usage — percentage of CPU time in user, system, idle, and wait states.  
- **Line 4:** Displays memory utilization — total, free, used, and memory used for buffers and caching.  
- **Line 5:** Shows swap (virtual memory) usage — total, free, and used swap, along with “avail Mem” (estimated available memory before swap usage).

---

#### Tasks Portion (Process Details)

The lower section lists processes in **12 columns**:

| Columns | Description                                             |
| ------- | ------------------------------------------------------- |
| 1–2     | **PID** (process ID) and **USER** (owner)               |
| 3–4     | **PR** (priority) and **NI** (nice value)               |
| 5–6     | **VIRT** (virtual memory) and **RES** (resident memory) |
| 7       | **SHR** (shared memory)                                 |
| 8       | **S** (process state)                                   |
| 9–10    | **%CPU** and **%MEM** usage                             |
| 11      | **TIME+** (CPU time in hundredths of a second)          |
| 12      | **COMMAND** (process name)                              |

---

### Interactive Commands in top

While inside **top**, you can use the following keys:

- **o** → Reorder (re-sequence) the process list  
- **f** → Add or remove displayed fields  
- **F** → Select the field to sort on  
- **h** → Display help information  

The **top** command is highly customizable — refer to its manual page (`man top`) for configuration details.


--- 
---

## Listing a Specific Process

You can list a PID of a specific process using **pidof** or **pgrep**

---
## Listing Process by User and Group Ownership

ps -u username : Displays processes from a specific user

ps -G groupname : Displays processes from a group

---

## Understanding Process Niceness and Priority

Linux is a **multitasking operating system**. It runs numerous processes on a single processor core by rapidly switching between them — this process is managed by the **scheduler**, which gives each process a small **time slice**. This creates the illusion of concurrent execution on one core.

---

### Niceness and Priority

Each process starts with a **priority**, determined by a numeric value called **niceness** (or *nice value*).  
There are **40 niceness levels**, ranging from **-20** (highest priority) to **+19** (lowest priority).

- **-20** → most favorable (highest priority)  
- **0** → default priority  
- **+19** → least favorable (lowest priority)

Higher niceness = lower priority.  
Lower niceness = higher priority.

Most system processes start at the default **niceness of 0**.  
A **child process** inherits the niceness of its **parent process**.

Regular users can only increase niceness (make processes *nicer*), while the **root user** can both raise or lower niceness values for any process.

---

### Managing Niceness and Priority

RHEL provides two commands:

- **nice** → launch a program at a custom niceness  
- **renice** → change the niceness of a running process  

---

#### Viewing Niceness

You can check the default niceness using:

```bash
nice
ps -el
ps -efl
```

The output shows:
- **PRI** (priority) in column 7  
- **NI** (niceness) in column 8  

Example:
- A niceness (**NI**) of `0` maps to a priority (**PR**) of `80`.  
- A niceness (**NI**) of `-20` maps to a priority (**PR**) of `60`.

---

### Niceness Display in ps vs top

The **ps** and **top** commands display niceness and priority differently:

- In **ps**: niceness `0–19` maps to priority `80–99`, and niceness `-20` maps near `60`.  
- In **top**: priority values are scaled differently (`0–20` instead of `60–80`).  

So, a priority shown as `80` in **ps** may appear as `20` in **top**.

---
## Controlling Processes with Signals

Linux uses **signals** to control or communicate with running processes.  
Signals can pause, terminate, or restart a process, and are sent using commands like **kill**, **pkill**, or **killall**.

---

### Common Signals

| Signal | Name    | Action                                      |
| ------ | ------- | ------------------------------------------- |
| 1      | SIGHUP  | Reloads config or disconnects from terminal |
| 2      | SIGINT  | Interrupts (Ctrl+c)                         |
| 9      | SIGKILL | Forcefully kills process                    |
| 15     | SIGTERM | Graceful termination (default)              |
| 18     | SIGCONT | Resume (like `bg`)                          |
| 19     | SIGSTOP | Suspend (like Ctrl+z)                       |
| 20     | SIGTSTP | Bring to foreground (`fg`)                  |

---

### Key Commands

- kill -l → list all signals  
- kill -9 PID→ forcefully kill by PID  
 -  pkill name→ kill by process name  
- killall name→ kill all matching processes  
- pgrep / pidof → find process IDs  

Regular users can signal their own processes; root can signal any.  
Use SIGTERM (15)for safe termination and SIGKILL (9) only when necessary.

---
## Job Scheduling

Job scheduling is taken care of b atd and cond

Files are located in /var/spool/cron and /etc/cond.d directories. 

For any additions or changes, neither daemon needs a restart.

---
## Controlling User Access

|                                   |                                   |                                                                 |
| --------------------------------- | --------------------------------- | --------------------------------------------------------------- |
| at.allow / cron.allow             | at.deny / cron.deny               | Impact                                                          |
| Exists, and contains user entries | Existence does not matter         | All users listed in allow files are permitted                   |
| Exists, but is empty              | Existence does not matter         | No users are permitted                                          |
| Does not exist                    | Exists, and contains user entries | All users, other than those listed in deny files, are permitted |
| Does not exist                    | Exists, but is empty              | All users are permitted                                         |
| Does not exist                    | Does not exist                    | No users are permitted                                          |
By default the deny files exist and are empty. And the allowe filles are non-existent. This opens up full access to using tools for all users

Exam TIP: One username is entered per line entry in an appropriate allow or deny file.

---
## Scheduler Log File

All activities for atd and cond services are logge d to the /var/log/cron file.

You will find information like time activity, process name and PID.a

## Using at

The **at** command schedules a one-time task run by the **atd** daemon.  
Jobs are saved in **/var/spool/at** and don’t require restarting the daemon.  

**Examples:**  
`at 1:15am`, `at noon`, `at 17:05 tomorrow`, `at now + 5 hours`, `at 3:00 10/15/23`  

**Run a file:**  
`at -f /path/script.sh now + 2 hours` → executes the file later and logs it.

---

## Using crontab

**crontab** schedules recurring tasks managed by the **crond** daemon.  
User crontabs are stored in **/var/spool/cron**, e.g., `/var/spool/cron/user1`.  
System-wide crontabs are in **/etc/crontab** and **/etc/cron.d** (root-only).  
Jobs are logged in **/var/log/cron**; no daemon restart needed.

**Main options:**  
- `crontab -e` → edit  
- `crontab -l` → list  
- `crontab -r` → remove  
- `crontab -u user` → modify another user’s crontab  

**Crontab Syntax:**  
`minute hour day month weekday command`  
- Minute: 0–59  
- Hour: 0–23  
- Day: 1–31  
- Month: 1–12 or jan–dec  
- Weekday: 0–7 (sun–sat)  
- Command: program or script to run  

Use `*` for “every,” commas for lists, dashes for ranges, and `/` for step values (e.g., `*/5` = every 5).  

**Example:**  
`0 3 * * 1 /home/user/backup.sh` → runs every Monday at 3:00 AM.
