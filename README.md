# 🖥️ MINI SHELL IMPLEMENTATION IN C

### Linux System Programming | Process Management | Command Execution

<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=22&pause=1000&color=00F5A0&center=true&vCenter=true&width=850&lines=Building+a+Linux+Shell+Using+C;Understanding+Process+Management;fork()+%7C+exec()+%7C+wait();From+User+Command+to+Process+Execution;Exploring+Linux+System+Programming" alt="Animated Mini Shell heading"/>

<br/>

<img src="https://img.shields.io/badge/Language-C-00599C?style=for-the-badge&logo=c"/>
<img src="https://img.shields.io/badge/Platform-Linux-FCC624?style=for-the-badge&logo=linux"/>
<img src="https://img.shields.io/badge/Domain-System%20Programming-00A98F?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Compiler-GCC-orange?style=for-the-badge"/>

**PURUSHOTHAMA K | EMBEDDED SOFTWARE ENGINEER ASPIRANT**

</div>

---

## `01 / PROJECT OVERVIEW`

### Building a Command-Line Interpreter from Scratch

The Mini Shell Implementation is a command-line application developed using C programming in a Linux environment.

The project explores how a command-line shell interacts with the operating system to process user commands and execute programs.

Instead of depending entirely on an existing shell, this project focuses on understanding the underlying system programming concepts involved in command execution.

**Engineering objective:** Understand how Linux creates processes, executes programs and manages parent-child process relationships using system calls.

### Core Capabilities

- Accept user commands through a terminal interface.
- Process command input and arguments.
- Create child processes using `fork()`.
- Execute external programs using `exec()`.
- Synchronize process execution using `wait()`.
- Understand parent-child process relationships.

*Additional shell features should be listed only when implemented in the actual source code.*

---

## `02 / SYSTEM ARCHITECTURE`

### From User Input to Linux Process Execution

```text
       ┌──────────────────────┐
       │      USER INPUT      │
       │                      │
       │      ls -l           │
       └──────────┬───────────┘
                  │
                  ▼
       ┌──────────────────────┐
       │    MINI SHELL        │
       │                      │
       │   Read Command       │
       │   Parse Arguments    │
       └──────────┬───────────┘
                  │
                  ▼
       ┌──────────────────────┐
       │    PROCESS CREATION  │
       │                      │
       │       fork()         │
       └──────────┬───────────┘
                  │
           ┌──────┴──────┐
           │             │
           ▼             ▼
    ┌────────────┐  ┌────────────┐
    │ PARENT     │  │ CHILD      │
    │ PROCESS    │  │ PROCESS    │
    │            │  │            │
    │ wait()     │  │ exec()     │
    │            │  │            │
    └──────┬─────┘  └──────┬─────┘
           │               │
           │               ▼
           │        ┌────────────┐
           │        │ COMMAND    │
           │        │ EXECUTION  │
           │        └──────┬─────┘
           │               │
           └───────┬───────┘
                   ▼
          ┌──────────────────┐
          │  RETURN TO SHELL │
          │  NEXT COMMAND    │
          └──────────────────┘
```

---

## `03 / HOW THE MINI SHELL WORKS`

### Understanding the Execution Lifecycle

**STEP 01 — COMMAND INPUT**

The user enters a command through the terminal.

Example:

```bash
ls -l
```

**STEP 02 — COMMAND PARSING**

The shell processes the input and separates the command from its arguments.

Example:

```text
Command:   ls
Argument:  -l
```

**STEP 03 — PROCESS CREATION**

The `fork()` system call creates a child process.

The parent and child continue execution in separate process contexts.

**STEP 04 — PROGRAM EXECUTION**

The child process uses an appropriate `exec()` family function to replace its process image with the requested program.

**STEP 05 — PROCESS SYNCHRONIZATION**

The parent uses `wait()` or the implemented wait variant to collect the child's termination status and synchronize execution.

**STEP 06 — NEXT COMMAND**

After the foreground command completes, the shell can return to the command prompt and accept another command.

---

## `04 / LINUX SYSTEM CALLS`

### The Core of Process Management

| System Call | Purpose | Application |
|---|---|---|
| `fork()` | Creates a child process | Process creation |
| `exec()` family | Replaces the process image | Program execution |
| `wait()` | Collects child process status | Synchronization |
| `getpid()` | Retrieves process ID | Process identification |
| `getppid()` | Retrieves parent process ID | Parent-child relationship |

### Understanding fork()

```c
pid_t pid = fork();

if (pid == 0)
{
    // Child process
}
else if (pid > 0)
{
    // Parent process
}
else
{
    // Fork failed
}
```

### Understanding exec()

```c
execvp(args[0], args);
```

The `execvp()` function attempts to execute the requested program using the argument list.

### Understanding wait()

```c
wait(NULL);
```

The parent process waits for a child process to terminate.

*These are illustrative examples of the system calls used in Linux programming.*

---

## `05 / PROCESS MANAGEMENT FLOW`

### Parent and Child Process Relationship

```text
              MINI SHELL
                  |
                  |
                fork()
                  |
          ┌───────┴────────┐
          |                |
          ▼                ▼
      PARENT             CHILD
          |                |
          |              exec()
          |                |
          |                ▼
          |         EXTERNAL PROGRAM
          |                |
        wait()             |
          |                |
          └───────┬────────┘
                  |
                  ▼
           SHELL CONTINUES
```

**Important concept:** `fork()` creates a new process, while `exec()` replaces the calling process image with another program. They perform different responsibilities.

---

## `06 / COMMAND EXECUTION EXAMPLE`

### What Happens When You Enter a Command?

User enters:

```bash
pwd
```

The Mini Shell:

1. Reads the command.
2. Identifies the command name.
3. Creates a child process.
4. Executes the `pwd` program in the child.
5. Waits for the child to finish.
6. Displays the next shell prompt.

Expected command output:

```text
/home/user/project
```

*The displayed path is an example; actual output depends on the working directory.*

---

## `07 / TECHNICAL CONCEPTS**

| Concept | Practical Application |
|---|---|
| Process Creation | Creating child processes |
| Process Execution | Running external commands |
| Parent-Child Relationship | Understanding process hierarchy |
| Process Synchronization | Waiting for process completion |
| System Calls | Interacting with the Linux kernel |
| Command Parsing | Processing user input |
| C Programming | Implementing shell logic |

---

## `08 / DEVELOPMENT ENVIRONMENT`

| Category | Technology |
|---|---|
| Programming Language | C |
| Operating System | Linux |
| Compiler | GCC |
| System Interface | Linux system calls |
| Development Domain | Linux System Programming |

---

## `09 / PROJECT STRUCTURE`

```text
Mini-Shell-Implementation/
│
├── src/
│   ├── main.c
│   ├── shell.c
│   └── shell.h
│
├── README.md
└── .gitignore
```

*Illustrative structure. Keep the repository synchronized with your actual source files.*

---

## `10 / BUILD AND EXECUTION`

### Prerequisites

- Linux operating system or Linux environment.
- GCC compiler.
- Basic C programming knowledge.

### Compilation

For a single source file:

```bash
gcc main.c -o minishell
```

For multiple source files:

```bash
gcc *.c -o minishell
```

### Execution

```bash
./minishell
```

Enter supported commands at the shell prompt to test command execution.

---

## `11 / ENGINEERING SKILLS DEVELOPED`

- Linux system programming fundamentals.
- Process creation and execution.
- Parent-child process management.
- System call understanding.
- Command-line application development.
- C programming and debugging.
- Process synchronization.
- Operating system fundamentals.

---

## `12 / FUTURE ENHANCEMENTS`

- [ ] Add command history.
- [ ] Improve command parsing.
- [ ] Add input and output redirection.
- [ ] Implement pipe support.
- [ ] Add background process execution.
- [ ] Improve signal handling.
- [ ] Add robust error handling.

---

## `13 / ABOUT THE DEVELOPER`

<div align="center">

### PURUSHOTHAMA K

**Embedded C | Linux Programming | Embedded Software Development**

Electrical and Electronics Engineering Graduate

Interested in Embedded Systems, Linux Internals, System Programming and Firmware Development.

[![GitHub](https://img.shields.io/badge/GitHub-My%20Profile-181717?style=for-the-badge&logo=github)](https://github.com/kpurushothama446-droid)

</div>

---

<div align="center">

### UNDERSTAND THE KERNEL. MANAGE THE PROCESS. BUILD THE SYSTEM.

**DESIGN | DEVELOP | DEBUG**

</div>
