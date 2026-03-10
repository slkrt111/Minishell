#  Minishell

<p align="center">
  <strong>A simplified Bash-like command language interpreter.</strong><br>
  <em>A complete command interpreter developed in C, focusing on process management and inter-process communication.</em>
</p>

---

## 📑 Table of Contents
* [Introduction](#introduction)
* [Features](#features)
* [Installation](#installation)
* [Usage](#usage)
* [Technical Implementation](#technical-implementation)

---

## 📝 Introduction
This project consists of recreating a minimalist shell. The goal is to understand the internal workings of a terminal in depth: reading input, parsing tokens, expanding variables, and executing processes via Unix system calls.


---

## ✨ Features

* **Interactive Prompt**: Display of a functional prompt with history management via `readline`.
* **Command Execution**:
    * Search in the **PATH** for system commands (`ls`, `grep`, `cat`, etc.).
    * Management of relative (`./script.sh`) and absolute (`/bin/ls`) paths.
* **Built-ins (Internal Commands)**:
    * `echo` with the `-n` option.
    * `cd`, `pwd`, `export`, `unset`, `env`, `exit`.
* **Redirections & Pipes**:
    * Input redirection `<` and **Here-doc** `<<`.
    * Output redirection `>` (overwrite) and `>>` (append).
    * **Pipes** `|` to connect the output of one command to the input of the next.
* **Signal Handling**:
    * `Ctrl-C`: Displays a new prompt.
    * `Ctrl-D`: Quits the shell (EOF).
    * `Ctrl-\`: Does nothing (matching Bash behavior).

---

## 🛠️ Installation

### 1. Clone the repository
```bash
git clone [https://github.com/your-username/minishell.git](https://github.com/your-username/minishell.git)
cd minishell
```

### 2. Compilation
The project uses a standard Makefile
```bash
make
```

## Usage
Once compiled, launch the executable
```bash
./minishell
```
Example of supported commands:
```bash
minishell> ls -l | grep ".c" > list.txt
minishell> echo $USER is using $SHLVL
minishell> export MY_VAR="42"
minishell> cat << END
> hello world
> END
```

## ⚙️ Technical Implementation
### 1. Lexing & Parsing
The command line is broken down into Tokens (words, operators, redirections). The parser ensures that quotes (' and ") are closed and correctly interpreted.

### 2. Expansion
Before execution, the shell replaces environment variables (e.g., $USER) with their actual values and handles the exit status code $?.

### 3. Execution Engine
Heavy use of Unix system calls:

fork(): Create child processes for each command.

pipe(): Create communication channels between processes.

dup2(): Redirect file descriptors (STDIN, STDOUT).

execve(): Replace the current process with the program to be executed.

### 4. Signal Handling
Using sigaction to handle keyboard interruptions and maintain the integrity of the prompt without memory leaks.




