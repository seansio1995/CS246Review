Linux
===

Linux Shell
---

Graphical Shell

	1. Example:
	2. Pros:
	3. Cons:

Command-line Shell

	1. Example:
	2. Pros:
	3. Cons:

```bash
echo $0
```

Linux File System
---

Tree Structure

	1. Files
	2. Directory

Root Directory:

Current Directory:

```bash
pwd
```

Absolute Path:

Relative Path:

```bash
cd
```

Special Directories

	1. .
	2. ..
	3. ~
	4. ~userid

```bash
ls
ls *.txt
```

```bash
cat
```
```
Ctrl+C
Ctrl+D
```

Input/Output Redirection
---

Each program is attached to 3 streams

Standard Input(stdin) | Program Process | Standard Output(stdout)(buffered)
--- | --- | ---
|  |  | **Standard Error(stderr)(not buffered)** |

```bash
myprog < in.txt > out.txt 2 > err.log
```

Pipes
---

Want to give the output of a program as an argument to another command/program

```bash
head -20 sample.txt | wc -w
```

```bash
echo Today is `date` and I am whoiam
echo Today is $(date) and "I am whoiam"
echo "Today is `date`" and "I am $(whoiam)"
echo 'Today is `date`' and 'I am $(whoiam)'
```

Pattern Matching within Text Files
---

egrep -- extended global regualr expression pattern (grep -E)

	* -i
	* a|b|c|d = [abcd] = [a-d]
	* [^abcd]
	* ?
	* *
	* ^
	* $
	* .
	* \
	* +

Permissions
---

```bash
ls -l
```

group: each user can belong to one or more groups, each file belongs to one group

type of file:

	* ordinary file
	* directory
	* symbolic link

three groups:

	* user
	* group
	* other

  | File | Directory 
--- | --- | ---
**r** | see the centents of the file (cat) | read the contents (ls) 
**w** | file contents can be modified | modify contents add/remove file
**x** | can execute a file | be able to navigate it (cd)

The owner is the only one with the ability to change file permissions. There is no permission that grants the ability to change file permissions.

```bash
chmod mode file
```

mode: ownership-class (u/g/o/a) operator(+/-/=) permission(r/w/x) 

Shell Variables
---

```bash
x=1
echo $1
```

1. To set a variable do not use $
2. To access the value of a variable, do use $ or ${}
3. Values for variables are always strings

Script
---

A file containing sequence of linux commands, executed as a program

1. need to tell the shell where the script is
2. script must have execute permission depending on who you are

Command line arguments
---

$i i-th argument

Example:
	1. A good password is not in the dictionary
	2. Print the number from 1 to $1

For loops

if statement

Testing
---

Numeric Ranges

	* check extremes
	* edge cases
	* corner cases
