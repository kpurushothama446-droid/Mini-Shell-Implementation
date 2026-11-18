# 🖥️ MINI SHELL IMPLEMENTATION IN C

### Linux System Programming | Process Management | Command Execution

<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=22&pause=1000&color=00F5A0&center=true&vCenter=true&width=850&lines=Building+a+Linux+Shell+Using+C;Understanding+Process+Management;fork()+%7C+execvp()+%7C+wait();From+User+Command+to+Process+Execution;Exploring+Linux+System+Programming" alt="Animated Mini Shell heading"/>

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

The Mini Shell is a command-line application developed using C programming in a Linux environment.

The project explores how a shell interacts with the Linux operating system to process user commands and execute external programs.

The application demonstrates the fundamental mechanism behind command execution using process creation, program replacement and process synchronization.

Rather than depending entirely on an existing shell, this implementation focuses on understanding the system calls that make command execution possible.

**Engineering objective:** Develop practical knowledge of Linux process management and system-level programming using C.

---

## `02 / PROJECT ARCHITECTURE`

### User-to-Kernel Interaction

```text
           USER
             |
             | Enter command
             ▼
      ┌───────────────┐
      │   MINI SHELL  │
      │               │
      │ Input Reading │
      │ Command Parse │
      └───────┬───────┘
              |
              ▼
       ┌─────────────┐
       │   fork()    │
       │             │
       │ Create Child│
       └──────┬──────┘
              |
        ┌─────┴──────┐
        |            |
        ▼            ▼
   ┌─────────┐  ┌──────────┐
   │ PARENT  │  │  CHILD   │
   │         │  │          │
   │ wait()  │  │ execvp() │
   │         │  │          │
   └────┬────┘  └────┬─────┘
        |            |
        |            ▼
        |      ┌────────────┐
        |      │  PROGRAM   │
        |      │ EXECUTION  │
        |      └─────┬──────┘
        |            |
        └──────┬─────┘
               |
               ▼
        NEXT SHELL PROMPT
```

This architecture demonstrates the basic foreground command execution lifecycle.

---

## `03 / HOW THE MINI SHELL WORKS`

### The Complete Execution Journey

**STEP 01 — COMMAND INPUT**

The shell displays a prompt and waits for the user to enter a command.

Example:

```bash
minishell$ ls -l
```

**STEP 02 — INPUT PROCESSING**

The entered command is read and processed.

The shell separates the command name and its arguments.

```text
Command  : ls
Argument : -l
```

**STEP 03 — PROCESS CREATION**

The `fork()` system call creates a child process.

The parent and child continue from the point where `fork()` returns, with different return values.

**STEP 04 — PROGRAM EXECUTION**

The child process uses the `exec()` family, such as `execvp()`, to execute the requested external program.

The child's previous process image is replaced by the new program.

**STEP 05 — PROCESS SYNCHRONIZATION**

The parent process uses `wait()` or the appropriate wait function to wait for the child process to terminate and collect its status.

**STEP 06 — RETURN TO PROMPT**

After foreground execution finishes, the shell continues its main loop and accepts the next command.

---

## `04 / CORE LINUX SYSTEM CALLS`

### Understanding the Operating System Interface

| System Call | Responsibility |
|---|---|
| `fork()` | Creates a child process |
| `execvp()` | Executes a program using command and argument vector |
| `wait()` | Waits for child process termination |
| `getpid()` | Returns current process ID |
| `getppid()` | Returns parent process ID |

### Process Creation — fork()

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
    // Process creation failed
}
```

### Program Execution — execvp()

```c
execvp(args[0], args);
```

The `execvp()` function searches for the executable using the PATH environment variable and executes it with the supplied arguments.

### Process Synchronization — wait()

```c
wait(NULL);
```

The parent waits for a child process to terminate.

**Important:** `fork()` creates a process, `execvp()` replaces the current process image, and `wait()` synchronizes the parent with child termination.

---

## `05 / PARENT AND CHILD PROCESS MANAGEMENT`

### Two Processes, Two Responsibilities

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
          |              execvp()
          |                |
          |                ▼
          |          NEW PROGRAM
          |                |
        wait()             |
          |                |
          └───────┬────────┘
                  |
                  ▼
          SHELL CONTINUES
```

### Parent Process

- Maintains the shell execution loop.
- Creates child processes.
- Waits for foreground child processes.
- Continues accepting user commands.

### Child Process

- Receives the command and arguments through the inherited process memory.
- Executes the requested program using an `exec()` function.
- Reports execution failure if the `exec()` call returns.

---

## `06 / COMMAND EXECUTION EXAMPLE`

### What Happens Behind a Simple Command?

User enters:

```bash
pwd
```

The Mini Shell processes the request:

1. Reads `pwd` from user input.
2. Processes the command name.
3. Creates a child process using `fork()`.
4. Executes the `pwd` program using an appropriate `exec()` function.
5. Parent waits for the child to finish.
6. Shell returns to the next prompt.

Example output:

```text
/home/user/projects
```

The actual output depends on the current working directory.

---

## `07 / TECHNICAL CONCEPTS`

| Concept | Practical Application |
|---|---|
| Process Creation | Creating child processes |
| Process Execution | Executing external programs |
| Parent-Child Relationship | Understanding process hierarchy |
| Process Synchronization | Waiting for child completion |
| System Calls | Interacting with the operating system |
| Command Parsing | Processing user input |
| C Programming | Implementing shell logic |

---

## `08 / DEVELOPMENT ENVIRONMENT`

| Category | Technology |
|---|---|
| Programming Language | C |
| Operating System | Linux |
| Compiler | GCC |
| System Interface | POSIX/Linux system calls |
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

*Illustrative structure. Update it to match the actual source files in the repository.*

---

## `10 / BUILD AND EXECUTION`

### Prerequisites

- Linux environment
- GCC compiler
- Basic C programming knowledge

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
