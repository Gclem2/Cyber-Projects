# Bash Notes

## Bash overview

- The bash shell identified by $ sign for normal users and # sign for root 
- Bash is can be used for variable manipulation, variable substitution, command substitution, input and output redirections, history substitution, tab completion, tilde substitution, alias substitution, metacharacters, pattern matching, filename globbing, quoting mechanisms, conditional execution, flow control, and shell scripting.
- The bash shell lives in /usr/bin/bash

## Shell and Environment Variables

## # Overview
A **variable** is a temporary storage space in memory that holds data used to customize the shell environment. Programs often reference these variables to function correctly.  
> When assigning values with spaces, enclose them in **quotation marks**.

---

## Types of Variables

### 1. **Local (Shell) Variables**
- Exist **only in the current shell**.
- **Not inherited** by programs or sub-shells.
- Used for shell-specific configuration.
- Example:
  ```bash
  myvar="Hello"
  echo $myvar
  ```

### 2. Environment Variable
* Exist in every shell
* Like a global variable programming
* Made with export
```bash
export VR1
```
3
## Setting and Unsetting Shell & Environment Variables

Shell and environment variables can be **set, unset, and viewed** at the command prompt or through programs. Built-in shell commands like `export`, `unset`, and `echo` are used to manage these variables.  

> **Tip:** Use **uppercase letters** for variable names to avoid conflicts with existing commands, files, or directories.

---

### Commands Overview

| **Action**                         | **Command**   | **Description**                                          |
| ---------------------------------- | ------------- | -------------------------------------------------------- |
| Define a **local variable**        | `VR1="value"` | Creates a variable `VR1` only in the current shell.      |
| View variable value                | `echo $VR1`   | Prints the value of `VR1`.                               |
| Export to **environment variable** | `export VR1`  | Makes `VR1` available to sub-shells and programs.        |
| Unset variable                     | `unset VR1`   | Removes the variable from the current shell environment. |

---

### Example: Define a Local Variable

```bash
# Define a local variable
VR1="RHEL9"

# View the value
echo $VR1

#Local variable
export VR1

#Global variable
bash

#Should be able to see value
```
* Put quotes around values for white spaces

## Command and Variable Substitution

The **primary command prompt**:
- Ends with `#` for the **root user**.
- Ends with `$` for **normal users**.  

Customizing your prompt can provide **useful information**, such as:
- Current user
- Hostname
- Current directory

This is managed through the **`PS1` environment variable**.

---

### Default PS1

```bash
echo $PS1
```

```bash 
\u@\h \W\$

```
# Input, Output, and Error Redirections

By default, programs:
- **Read input** from the keyboard.
- **Write output** to the terminal.
- **Display errors** on the terminal.

Bash treats input, output, and errors as **streams**, allowing you to **redirect** them to/from files or other destinations.

---

##  Standard Streams

| **Stream**               | **Default Location** | **Symbol / FD** |
| ------------------------ | -------------------- | --------------- |
| Standard Input (stdin)   | Keyboard             | `<` or `0`      |
| Standard Output (stdout) | Terminal             | `>` or `1`      |
| Standard Error (stderr)  | Terminal             | `2>` or `2`     |

**Exam Tip:** Commands often require redirection of output or errors to files.

---

## Redirecting Standard Input

Input redirection allows a command to **read from a file** instead of the keyboard.

**Syntax:**
```bash
command < input_file

```
## Input, Output, and Error Redirections

By default, programs:
- **Read input** from the keyboard.
- **Write output** to the terminal.
- **Display errors** on the terminal.

Bash treats input, output, and errors as **streams**, allowing you to **redirect** them to files or other destinations.

---

### Standard Streams

| **Stream** | **Default Location** | **Symbol / FD** |
|------------|-------------------|----------------|
| Standard Input (stdin) | Keyboard | `<` or `0` |
| Standard Output (stdout) | Terminal | `>` or `1` |
| Standard Error (stderr) | Terminal | `2>` or `2` |

---

### Redirecting Standard Input

Input redirection lets a command **read from a file** instead of the keyboard.

```bash
cat < /etc/redhat-release
```

## History Substitution

### Overview
History substitution (also known as **command history** or **history expansion**) is a Bash feature that saves all executed commands in chronological order. It allows users to retrieve, modify, and rerun previous commands. History is stored both in **system memory** and a file in the user’s home directory.

---

### Key Variables

| Variable         | Description                                                          | Default           |
| ---------------- | -------------------------------------------------------------------- | ----------------- |
| **HISTFILE**     | Specifies the name and location of the history file.                 | `~/.bash_history` |
| **HISTSIZE**     | Maximum number of commands stored in memory for the current session. | —                 |
| **HISTFILESIZE** | Maximum number of commands stored in the history file.               | —                 |

- `HISTSIZE` and `HISTFILESIZE` are usually set to the same value.  
- View variable values:  
  ```bash
  echo $HISTFILE $HISTSIZE $HISTFILESIZE

```
### The `history` Command

The `history` command displays or reruns previously executed commands.  
It retrieves data from system memory and the `.bash_history` file.

### Common Uses

| Action                | Command         | Description                                      |
| --------------------- | --------------- | ------------------------------------------------ |
| Show full history     | `history`       | Displays all stored commands.                    |
| Show last 10 commands | `history 10`    | Lists recent commands.                           |
| Rerun by line number  | `!15`           | Executes the command at line 15.                 |
| Rerun by prefix       | `!ch`           | Runs the most recent command starting with “ch”. |
| Rerun by keyword      | `!?grep`        | Runs the most recent command containing “grep”.  |
| Remove an entry       | `history -d 24` | Deletes entry 24.                                |
| Repeat last command   | `!!`            | Executes the most recent command a               |
## Enabling and Disabling History

- Disable command history:

```bash
set +o history

#reenable history

set -o history
```

## Editing at the command line

|                      |                                                       |
| -------------------- | ----------------------------------------------------- |
| Key Combinations     | Action                                                |
| Ctrl+a / Home        | Moves the cursor to the beginning of the command line |
| Ctrl+e / End         | Moves the cursor to the end of the command line       |
| Ctrl+u               | Erase the entire line                                 |
| Ctrl+k               | Erase from the cursor to the end of the command line  |
| Alt+f                | Moves the cursor to the right one word at a time      |
| Alt+b                | Moves the cursor to the left one word at a time       |
| Ctrl+f / Right arrow | Moves the cursor to the right one character at a time |
| Ctrl+b / Left arrow  | Moves the cursor to the left one character at a time  |

## Tab Completion

### Overview
**Tab completion** (also known as **command line completion**) is a Bash shell feature that allows quick and automatic completion of file, directory, or command names.

When you type the first few characters of a name and press the **Tab** key twice, Bash attempts to complete the entry automatically.

---

### How It Works
1. **Single Match** – If there’s only one possible match, Bash completes the name automatically.  
2. **Multiple Matches** – If several possibilities exist, Bash completes up to the shared portion and lists all possible matches.  
3. **Narrowing Down** – Type more characters and press **Tab** again to refine the results.  
4. **Execution** – Once the correct name appears, press **Enter** to execute the command or open the file/directory.

---

### Benefits
- Saves time typing long names.
- Reduces typos and command errors.
- Helps discover available files, directories, and commands.

## Tilde Substitution

### Overview
**Tilde substitution** (or **tilde expansion**) is a Bash feature that interprets words beginning with the tilde character (`~`). It simplifies navigation by automatically expanding `~` to specific directory paths based on context.

---

### Rules

1. **Standalone `~`**
   - Refers to the current user’s **home directory** (`$HOME`).
   - Example:  
     ```bash
     echo ~
     ```
     Displays the path to the user’s home directory.

2. **`~+` (Plus Sign)**
   - Refers to the **current working directory**.
   - Example:  
     ```bash
     echo ~+
     ```
     Outputs the current directory path.

3. **`~-` (Hyphen)**
   - Refers to the **previous working directory**.
   - Example:  
     ```bash
     echo ~-
     ```
     Displays the last directory visited.

4. **`~username`**
   - Refers to the **home directory** of the specified user.
   - Example:  
     ```bash
     echo ~user1
     ```
     Expands to `/home/user1`.

---

## Usage with Commands

- **Change directories using `~`:**
  ```bash
  cd ~user1
```
* Navigate as root
```bash
cd ~user1000

# cd as user
sudo cd ~user1
```
## Alias Substitution

### Overview
**Alias substitution** (also known as **command aliasing** or simply **alias**) allows users to define shortcuts for long or complex commands. When an alias is used, the shell executes the full command or command set associated with it. This feature improves efficiency and reduces typing effort.

---

### Viewing Existing Aliases
The Bash shell includes predefined aliases that are automatically set during login.

- View all current aliases:
  ```bash
  alias
```
Many predefined aliases use color options (e.g., `--color=auto`) to display output with highlighted filenames and patterns.

---

## Common Predefined Aliases

| Alias  | Value   | Description                        |
| ------ | ------- | ---------------------------------- |
| **cp** | `cp -i` | Copies files in interactive mode.  |
| **ll** | `ls -l` | Lists files in long format.        |
| **mv** | `mv -i` | Moves files in interactive mode.   |
| **rm** | `rm -i` | Removes files in interactive mode. |

## Creating a new Alias

```bash
alias name = 'command'
```

If  an alias  name matches an existing command the alias takes prescedence

To use original command :

```bash
\rm  filename
```
pre fix it with a \

### Removing an Alias

To remove an alias use:

```bash
unalias search rm
```

To remove all alias's

```bash
unalias -a
```


## Meta Characters and Wildcard characters 

### Overview
**Metacharacters** are special symbols that carry specific meanings in the Bash shell. They are commonly used for **pattern matching**, **filename expansion (globbing)**, and **regular expressions**.

Common metacharacters include:  
`$  ^  .  *  ?  |  <  >  { }  [ ]  ( )  +  !  ;  \`

This section focuses on the metacharacters used for **pattern matching**: `*`, `?`, `[]`, and `!`, collectively known as **wildcard characters**.

---

### The `*` Character

- Matches **zero or more characters**, except the leading period (`.`) in hidden files.

### Examples
```bash
ls /etc/ma*
```

| Character | Meaning                         | Example        | Description                        |
| --------- | ------------------------------- | -------------- | ---------------------------------- |
| `*`       | Matches zero or more characters | `ls *.txt`     | All `.txt` files                   |
| `?`       | Matches exactly one character   | `ls file?.txt` | `file1.txt`, `fileA.txt`, etc.     |
| `[]`      | Matches a specific set or range | `ls [a-c]*`    | Files starting with a, b, or c     |
| `[! ]`    | Excludes a set or range         | `ls [!a-c]*`   | Files not starting with a, b, or c |

---
## Piping Output of One Command as Input to Another

### Overview
The **pipe** (`|`) is a Bash operator used to send the **output of one command** as the **input to another**.  
It allows multiple commands to be chained together to process data efficiently.  
You can use the pipe operator **as many times as needed** in a single command.  
On most keyboards, the pipe symbol (`|`) shares a key with the backslash (`\`).

---

### Basic Usage
#### Example 1: View Long Listings One Page at a Time
```bash
ls -l /etc | less
```


---

# Quoting Mechanisms in Bash

Metacharacters have special meanings in the shell. To use them as regular characters, Bash provides **three quoting mechanisms** that disable their special interpretation:

---

### 1. Prefixing with a Backslash `\`

The **backslash** (also called the *escape character*) tells the shell to treat the following character **literally**, ignoring any special meaning it normally has.

**Example:**
```bash
echo \$LOGNAME

Output:

$LOGNAME
```

If a file is named *, you must escape it to avoid interpreting * as a wildcard:

```bash
rm \*
```

### 2. Enclosing within Single Quotes ' '

Single quotes instruct the shell to interpret everything inside literally — no variable expansion or command substitution occurs.

Example:

echo '$LOGNAME'

Output:

$LOGNAME

### 3. Enclosing within Double Quotes " "

Double quotes mask the meaning of all special characters except:

    The backslash (\)

    The dollar sign ($)

    The backtick (`)

These retain their special meanings inside double quotes.

Example:

```
echo "$LOGNAME"

Output:

```bash
your_username
```
## Shell Startup Files

When you modify your shell environment (e.g., setting variables or customizing the prompt), you can **save these changes in startup files** so they persist across sessions.  

These files are executed (or *sourced*) by the shell after user authentication — before the command prompt appears — and can contain **variables, aliases, functions, and scripts**.

---

### Types of Shell Startup Files

### 1. System-Wide Startup Files
- Apply to **all users** on the system.  
- Located in directories like `/etc/`.  
- Common examples:
  - `/etc/profile`
  - `/etc/bashrc`

---

### 2. Per-User Startup Files
- Apply only to the **specific user**.  
- Stored in the user’s **home directory** (e.g., `~`).  
- Common examples:
  - `~/.bash_profile`
  - `~/.bash_login`
  - `~/.profile`
  - `~/.bashrc`

---

### Purpose
These files allow users (and administrators) to:
- Customize **the shell prompt** (e.g., `PS1`)
- Define **environment variables**
- Create **aliases and functions**
- Run **scripts automatically** at login

---

### Tip
To ensure your environment setup is always loaded:
- Put **login-specific settings** in `~/.bash_profile`
- Put **interactive shell settings** (like aliases and functions) in `~/.bashrc`


# System-wide Shell Startup Files

System-wide startup files configure the **general environment for all users** at login.  
They are located in the `/etc` directory and are maintained by the system administrator.  
These files can be modified to include **common environment settings, functions, and customizations**.

---

## Key System-wide Startup Files

| File | Description |
|------|-------------|
| **/etc/bashrc** | Defines functions and aliases, sets `umask` for non-login shells, establishes the command prompt, and may include settings from scripts in `/etc/profile.d`. |
| **/etc/profile** | Sets common environment variables (`PATH`, `USER`, `LOGNAME`, `MAIL`, `HOSTNAME`, `HISTSIZE`, `HISTCONTROL`), establishes `umask` for login shells, and executes scripts in `/etc/profile.d`. |
| **/etc/profile.d** | Directory containing scripts for bash shell users that are executed by `/etc/profile`. |

---

## Notes
- Any script in `/etc/profile.d` can be **edited or updated**.  
- Users or administrators can also **create new scripts** in this directory to define custom environment settings or functions for all users.
# Per-user Shell Startup Files

Per-user startup files **override or modify system-wide defaults** and allow individual users to customize their shell environment.  
By default, files from `/etc/skel` are copied into new user home directories, including `.bashrc`, `.bash_profile`, and `.bash_logout`.

Users can also **create additional files** in their home directories to set more environment variables or shell properties.

---

## Key Per-user Startup Files

| File / Directory | Description |
|-----------------|-------------|
| **.bashrc** | Defines functions and aliases. Sources global definitions from `/etc/bashrc`. |
| **.bash_profile** | Sets environment variables and sources `.bashrc` to load functions and aliases. |
| **.gnome2/** | Stores environment settings for GNOME desktop (if installed). |

---

## Execution Order

The shell executes startup files in the following order:
1. `/etc/profile`  
2. `.bash_profile`  
3. `.bashrc`  
4. `/etc/bashrc`  

> **Exam Tip:** To make per-user settings persistent, add them to the appropriate per-user startup file.

---

## Logout File

- **.bash_logout** in the user’s home directory is executed when the user **logs out** or **leaves the shell**.  
- Can be customized to run commands on logout, such as clearing the screen or cleaning temporary files.
