# Chapter 21: Shell Scripting

## Overview
Text files containing Linux commands, control structures, and comments for automating repetitive tasks

## Key Characteristics
- **Interpreted**: Executed line by line by shell (no compilation needed)
- **Executable**: Run directly from command prompt
- **Purpose**: Automate simple to complex tasks

## RHCSA Objectives
- **12**: Conditionally execute code (`if`, `test`, `[]`)
- **13**: Use looping constructs (`for`) to process files/input
- **14**: Process script inputs (`$1`, `$2`, etc.)
- **15**: Process output of shell commands within scripts

## Topics Covered
1. Shell script basics
2. System information scripts
3. Shell and environment variables
4. Command substitution
5. Special and positional parameters
6. Script execution and debugging
7. Exit codes and test conditions
8. Logical constructs:
   - `if-then-fi`
   - `if-then-else-fi`
   - `if-then-elif-fi`
9. Arithmetic test conditions
10. Looping constructs: `for-do-done`

## Learning Approach
Progress from simple to complex scripts with examples and analysis

---
# Shell Scripts

## Definition
Text files containing Linux commands and control structures to automate tasks

## Common Use Cases
- Package and user management
- Partition and file system administration
- File system utilization monitoring
- Log file trimming
- File archiving/compression
- Finding/removing unnecessary files
- Database service management
- Report generation

## How Scripts Work
- **Interpreted**: Executed line by line by shell
- **Sequential**: Commands run in order listed
- **Error handling**: Error messages printed to screen if encountered

## Script Components
1. **Commands**: Executed as if typed at command prompt
2. **Control structures**: Conditional and looping constructs
3. **Comments**: Author, date, purpose, usage info

## Best Practices

### Shell
Scripts in this chapter use **bash** (portable to other shells with minor modifications)

### Editor
Use **vim** for practice opportunity

### Line Numbering
```bash
nl script.sh  # Enumerate lines for easier debugging
```

### Storage Location
```bash
/usr/local/bin/  # Default PATH location for all users
```

## Key Points
- No compilation required (interpreted)
- Each line executes like a command prompt entry
- Errors displayed during execution
---
# Executing a Script

## Default Permissions Issue
- Default umask (0022): read/write for owner, read-only for others
- **No execute permission** by default

## Add Execute Permission
```bash
chmod +x /usr/local/bin/sysinfo.sh
# Or: chmod 755 /usr/local/bin/sysinfo.sh
```

Now any user can execute the script

## Run Script
```bash
# Method 1: By name (if in PATH)
sysinfo.sh

# Method 2: Full path
/usr/local/bin/sysinfo.sh

# Method 3: Explicit interpreter
bash /usr/local/bin/sysinfo.sh
```

## Example Output
Script executes commands sequentially as written:
- `hostnamectl`: Shows hostname, platform type (physical/virtual), hypervisor, OS name/version, kernel version, hardware architecture
- Other commands execute in order

## Key Points
- Must add execute permission before running
- Can execute by name if in PATH directory
- Output displays results of each command in sequence
---
# Debugging a Script

## Debug Methods

### Method 1: Modify Shebang
```bash
#!/bin/bash -x
```
Add `-x` flag to script's first line

### Method 2: Execute with Debug Flag
```bash
bash -x /usr/local/bin/sysinfo.sh
```

## Debug Output Format
```
+ command_from_script
actual_output
+ next_command
next_output
```

**Features**:
- Lines prefixed with `+` show actual script commands
- Followed by command execution results
- Shows line numbers if errors occur

## What Debug Mode Reveals
- Path issues
- Command name errors
- Special character problems
- Execution flow

## Example Test
```bash
# Change valid command to invalid
echo "test"  →  iecho "test"

# Run in debug mode
bash -x script.sh
# Will show error at specific line
```

## Key Points
- Use `-x` for line-by-line execution trace
- Quickly identify where script fails
- Shows exact command being executed
- Displays line numbers for errors
---
# Script02: Using Local Variables

## Variable Types Recap
1. **Local (shell/private)**: Available only within current shell/script
2. **Environment**: Available to shell and child processes

Both usable in scripts and command line

## Example Script: use_var.sh
```bash
#!/bin/bash
SYSNAME=$(hostname)
echo "System name is: $SYSNAME"
```

### Make Executable and Run
```bash
chmod +x use_var.sh
./use_var.sh
# Output: System name is: server30
```

### Check Variable After Script
```bash
echo $SYSNAME
# Output: (empty/nothing)
```

## Key Points
- **Local variables**: Exist only during script execution
- **Not accessible** after script completes
- Variable scope limited to script's shell
- Dies when script exits

## Variable Definition
```bash
VARIABLE_NAME=value  # No spaces around =
```

---
# Script04: Using Command Substitution

## Overview
Store command output in variables during script execution

## Two Syntax Methods

### Method 1: `$(command)`
```bash
VARIABLE=$(command)
```

### Method 2: `` `command` ``
```bash
VARIABLE=`command`
```
**Note**: Use backticks (`` ` ``) located with `~` key

## Example Script: cmd_out.sh
```bash
#!/bin/bash
# Method 1: $() syntax
SYSNAME=$(hostname)

# Method 2: backticks syntax
KERNVER=`uname -r`

echo "System name: $SYSNAME"
echo "Kernel version: $KERNVER"
```

### Execute
```bash
chmod +x cmd_out.sh
./cmd_out.sh

# Output:
# System name: server30
# Kernel version: 5.14.0-362.el9.x86_64
```

## Key Points
- Both methods capture command output
- `$()` preferred (easier to nest, more readable)
- Backticks work but harder to nest
- Store output in variables for later use
---
# Understanding Shell Parameters

## Parameter Types

### 1. Variable
Parameter that holds a **name**  
Example: `MYVAR="value"`

### 2. Special Parameter
Parameter that holds a **special character**

| Parameter | Description |
|-----------|-------------|
| `$0` | Script/command name itself |
| `$#` | Count of supplied arguments |
| `$*` or `$@` | All arguments |
| `$$` | PID of current process |

### 3. Positional Parameter
Parameter that holds **one or more digits** (except 0)  
Also called **command line arguments**

| Parameter | Description |
|-----------|-------------|
| `$1` | First argument |
| `$2` | Second argument |
| `$3` | Third argument |
| `${10}` | Tenth argument (use `{}` for 10+) |

## Key Points
- Position determined by location relative to script name
- Use `${}` for positional parameters beyond 9
- All parameters expanded using `$` sign
- Similar to variable and command substitution syntax
---

![](../RHCSA_Labs/attachment/Pasted%20image%2020260126175451.png)
# Script05: Using Special and Positional Parameters

## Example Script: com_line_arg.sh
```bash
#!/bin/bash
echo "Script name: $0"
echo "All arguments: $@"
echo "Total count: $#"
echo "First argument: $1"
echo "Script PID: $$"
```

### Execute with Arguments
```bash
chmod +x com_line_arg.sh
./com_line_arg.sh arg1 arg2 arg3 arg4
```

### Output
```
Script name: ./com_line_arg.sh
All arguments: arg1 arg2 arg3 arg4
Total count: 4
First argument: arg1
Script PID: 12345
```

## Parameter Summary
- `$0`: Script name
- `$@`: All arguments
- `$#`: Argument count
- `$1`: First argument
- `$$`: Process ID of script
---
# Script06: Shifting Command Line Arguments

## Overview
`shift` command moves arguments one position left; first argument value is lost

## Example Script: com_line_arg_shift.sh
```bash
#!/bin/bash
echo "All arguments: $@"
echo "First argument: $1"

shift
echo "After 1st shift, first argument: $1"

shift
echo "After 2nd shift, first argument: $1"

shift
echo "After 3rd shift, first argument: $1"
```

### Execute
```bash
chmod +x com_line_arg_shift.sh
./com_line_arg_shift.sh arg1 arg2 arg3 arg4
```

### Output
```
All arguments: arg1 arg2 arg3 arg4
First argument: arg1
After 1st shift, first argument: arg2
After 2nd shift, first argument: arg3
After 3rd shift, first argument: arg4
```

## Multiple Shifts
```bash
shift    # Shift by 1 (default)
shift 2  # Shift by 2 positions
shift 3  # Shift by 3 positions
```

## Key Points
- Each shift moves arguments left by 1 position
- First argument lost after each shift
- `$1` gets new value after each shift
- Can shift multiple times with count argument
---
# Logical Constructs

## Overview
Control script flow using test conditions (true/false) instead of line-by-line execution

## Available Constructs
1. **if-then-fi** (with variations) - Covered in this chapter
2. **case** - Beyond scope

## Prerequisites
Before example scripts, need to understand:
- Exit codes
- Test conditions

Both used in logical construct examples

---
# Exit Codes

## Definition
Value returned by command upon completion based on outcome

**Also called**: Exit values, return codes

## Exit Code Values
- **0**: Success
- **Non-zero**: Failure/error

## Storage
Stored in special parameter **`?`**

## Examples

### Successful Command
```bash
pwd
echo $?
# Output: 0 (success)
```

### Failed Command
```bash
man
echo $?
# Output: non-zero (failed - missing argument)
```

## Usage in Scripts
Define exit codes at different script locations to:
- Aid debugging
- Identify exact termination point
---
# Test Conditions

## Usage
Decide next action in logical constructs using `test` command or `[ ]`

## Integer Tests
| Operator | Description |
|----------|-------------|
| `-eq` | Equal |
| `-ne` | Not equal |
| `-lt` | Less than |
| `-gt` | Greater than |
| `-le` | Less than or equal |
| `-ge` | Greater than or equal |

## String Tests
| Operator | Description |
|----------|-------------|
| `=` | Strings identical |
| `!=` | Strings not identical |
| `-z` or `-l` | String length is zero |
| `-n` or `string` | String length is non-zero |

## File Tests
| Operator | Description |
|----------|-------------|
| `-b` / `-c` | Block/character device |
| `-d` / `-f` | Directory/regular file |
| `-e` / `-s` | Exists/non-empty |
| `-L` | Symbolic link |
| `-r` / `-w` / `-x` | Readable/writable/executable |
| `-u` / `-g` / `-k` | setuid/setgid/sticky bit set |
| `file1 -nt file2` | file1 newer than file2 |
| `file1 -ot file2` | file1 older than file2 |

## Logical Operators
| Operator | Description | Example |
|----------|-------------|---------|
| `!` | NOT | `[ ! -f file ]` |
| `-a` or `&&` | AND (both true) | `[ -b file && -r file ]` |
| `-o` or `||` | OR (either/both true) | `[ x == 1 -o y == 2 ]` |

---
# The if-then-fi Construct

## Overview
Evaluates condition; executes action if true, otherwise exits construct

## Syntax
```bash
if condition
then
    action
fi
```

## Flow
1. Check condition
2. **If true**: Execute action
3. **If false**: Skip action
4. Exit construct (fi)

## Key Points
- Begins with `if`
- Ends with `fi` (reverse of if)
- Action only executes when condition is true
---
# Script07: The if-then-fi Construct

## Example Script: if_then_fi.sh
```bash
#!/bin/bash
if [ $# -ne 2 ]
then
    echo "Error: Two arguments required"
    exit 2
fi
echo "Success: Two arguments provided"
```

## Test Cases

### Without Two Arguments
```bash
./if_then_fi.sh arg1
# Output: Error: Two arguments required

echo $?
# Output: 2 (custom exit code from script)
```

### With Two Arguments
```bash
./if_then_fi.sh arg1 arg2
# Output: Success: Two arguments provided

echo $?
# Output: 0 (success)
```

## Key Points
- `$#` contains argument count
- Custom exit code (2) set for error condition
- Exit code 0 for success (default)
- Script exits immediately on error condition
---
# Script08: The if-then-else-fi Construct

## Example Script: if_then_else_fi.sh
```bash
#!/bin/bash
if [ $1 -ge 0 ]
then
    echo "$1 is positive"
else
    echo "$1 is negative"
fi
```

## Test Cases

### Positive Integer
```bash
./if_then_else_fi.sh 5
# Output: 5 is positive
```

### Negative Integer
```bash
./if_then_else_fi.sh -3
# Output: -3 is negative
```

### Non-Integer Value
```bash
./if_then_else_fi.sh abc
# Likely produces error (not handled in this basic script)
```

## Key Points
- Tests if `$1` (first argument) is greater than or equal to 0
- True path: positive message
- False path: negative message
- Does not validate input type (assumes integer)
---
# Script10: The if-then-elif-fi Construct (Example 2)

## Example Script: ex200_ex294.sh
```bash
#!/bin/bash
if [ "$1" = "ex200" ]
then
    echo "RHCSA exam"
elif [ "$1" = "ex294" ]
then
    echo "RHCE exam"
else
    echo "Usage: Acceptable values are ex200 and ex294"
fi
```

**Note**: White spaces required in conditions: `[ "$1" = "ex200" ]`

## Test Cases

### Valid Argument: ex200
```bash
./ex200_ex294.sh ex200
# Output: RHCSA exam
```

### Valid Argument: ex294
```bash
./ex200_ex294.sh ex294
# Output: RHCE exam
```

### Invalid/No Argument
```bash
./ex200_ex294.sh random
# Output: Usage: Acceptable values are ex200 and ex294
```

## Key Points
- String comparison using `=`
- Must quote `$1` for proper string handling
- White spaces required inside `[ ]`
- `else` catches all other inputs

## EXAM TIP
Good understanding of logical statements is important

---
# Looping Constructs

## Purpose
Perform tasks on multiple elements or repeat until condition met

**Example**: Initialize multiple disks for LVM with one loop instead of manual commands

## Available Loops

### 1. for-do-done (foreach)
Iterates through list until exhausted - **Covered in this chapter**

### 2. while-do-done
Runs repeatedly until condition becomes **false** - Out of scope

### 3. until-do-done
Runs repeatedly until condition becomes **true** - Out of scope

## Key Points
- Automates repetitive tasks
- Processes multiple items efficiently
- Condition-based execution control
---
# Test Conditions (Arithmetic)

## let Command
Evaluates conditions in loops; compares variable against value

**Alternative syntax**: Use `(( ))` or `" "` instead of explicit `let`

## Arithmetic Operators

| Operator | Description |
|----------|-------------|
| `!` | Negation |
| `+` `-` `*` `/` | Addition, subtraction, multiplication, division |
| `%` | Remainder (modulo) |
| `<` `<=` | Less than, less than or equal |
| `>` `>=` | Greater than, greater than or equal |
| `=` | Assignment |
| `==` `!=` | Equality, non-equality comparison |

## Usage in Loops
Variable value altered at each iteration; condition checked before continuing

## Examples
```bash
let "count = count + 1"
(( count++ ))
[ $count -lt 10 ]
```
---
# The for Loop

## Overview
Iterates through array of elements; processes each until list exhausted

## Syntax
```bash
for VAR in list
do
    action
done
```

## Flow
1. Assign first element from list to VAR
2. Execute action
3. Assign next element to VAR
4. Repeat until list exhausted
5. Exit loop

## Key Points
- Processes each element sequentially
- Variable holds current element value
- Continues until all elements consumed
---
# Script11: Print Alphabets Using for Loop

## Example Script: for_do_done.sh
```bash
#!/bin/bash
COUNT=0
for LETTER in {A..Z}
do
    echo "Letter: $LETTER"
    COUNT=`expr $COUNT + 1`
done
echo "Total letters: $COUNT"
```

## Key Elements
- **{A..Z}**: Range expansion (no spaces)
- **LETTER**: Variable holding current letter
- **expr**: Arithmetic processor for incrementing COUNT
- **COUNT**: Tracks iteration number

## Output
```
Letter: A
Letter: B
Letter: C
...
Letter: Z
Total letters: 26
```

## Key Points
- Brace expansion `{A..Z}` generates A through Z
- No spaces in range: `{A..Z}` not `{ A .. Z }`
- `expr` used for arithmetic operations
- Loop runs 26 times (once per letter)
---
