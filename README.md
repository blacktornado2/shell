# 🐚 Shell & Terminal — Fundamentals Guide

> A comprehensive reference for understanding the shell, terminal, configuration files, and core concepts every developer should know.

---

## 📚 Table of Contents

- [What is a Shell?](#what-is-a-shell)
- [Shell Examples](#shell-examples)
- [What is a Terminal?](#what-is-a-terminal)
- [Terminal vs Shell](#terminal-vs-shell)
- [How It All Flows](#how-it-all-flows)
- [Checking Your Shell](#checking-your-shell)
- [Login Shell vs Interactive Shell](#login-shell-vs-interactive-shell)
- [.rc Configuration Files](#rc-configuration-files)
- [Environment Variables](#environment-variables)
- [PATH](#path--the-most-important-environment-variable)
- [Aliases](#aliases)
- [Shell Functions](#shell-functions)
- [stdin, stdout, stderr](#stdin-stdout-stderr--standard-streams)
- [Pipes](#pipes)
- [Command Chaining](#command-chaining-operators)
- [History](#history)
- [Built-in Commands](#useful-shell-built-in-commands)
- [Quoting Rules](#quoting-rules)
- [Startup File Quick Reference](#startup-file-quick-reference)

---

## What is a Shell?

A **shell** is a program that provides an interface between the user and the operating system. It interprets user commands and passes them to the OS kernel for execution.

### Shell Examples

| Shell | Full Name | Notes |
|-------|-----------|-------|
| `sh` | Bourne Shell | Original UNIX shell |
| `bash` | Bourne Again SHell | Default shell on Linux |
| `zsh` | Z Shell | Enhanced bash + extra features; **default on macOS** |
| `fish` | Friendly Interactive Shell | User-friendly, autosuggestions, syntax highlighting |
| `PowerShell` | PowerShell Core | Cross-platform (Windows/Linux/macOS), object-based, from Microsoft |
| `Windows PowerShell` | — | Legacy predecessor to PowerShell Core; Windows only |
| `cmd` | Command Prompt | Legacy Windows shell |

### Graphical Shells (GUI-based)

- **Windows Explorer** — Windows
- **Aqua** — macOS GUI shell

### Using PowerShell on macOS / Linux

```sh
# 1. Install PowerShell (e.g., via Homebrew on macOS)
brew install --cask powershell

# 2. Launch it
pwsh
```

---

## What is a Terminal?

A **terminal** is a program that provides a text-based interface to interact with the shell (and through it, the OS kernel).

| Platform | Terminal Apps |
|----------|--------------|
| macOS | Terminal.app, iTerm2 |
| Linux | GNOME Terminal, Konsole |
| Windows | Windows Terminal, PowerShell, cmd |

---

## Terminal vs Shell

| | Role |
|---|---|
| **Terminal** | The app / interface (what you see) |
| **Shell** | The command processor running inside it |

> **Analogy:** The terminal is like a **car dashboard** (the interface you interact with). The shell is like the **engine** (doing the actual work when you drive).

---

## How It All Flows

```
User
  │
  ▼
Terminal        (Interface — e.g. iTerm2, Windows Terminal)
  │
  ▼
Shell           (Interpreter — e.g. zsh, bash)
  │
  ▼
OS Kernel       (System Control)
  │
  ▼
Hardware        (Execution)
```

---

## Checking Your Shell

```sh
# macOS / Linux — print the current running shell
ps -p $$

# Print the default login shell
echo $SHELL
```

**Windows — identify by prompt style:**

| Shell | Prompt Looks Like |
|-------|-------------------|
| `cmd.exe` | `C:\Users\John>` |
| `PowerShell` | `PS C:\Users\John>` |
| `Git Bash` | `ankit@ankitsPC MINGW64 ~` |

---

## Login Shell vs Interactive Shell

| Type | When it starts | Config files read |
|------|---------------|-------------------|
| **Login Shell** | Logging into a machine (SSH, macOS Terminal on open) | `.bash_profile`, `.zprofile`, `.profile` |
| **Interactive Shell** | Any shell you type commands into | `.bashrc`, `.zshrc` |
| **Non-interactive Shell** | Running scripts | Config files are generally **not** sourced |

**How they chain (bash):**

```
Login Shell        →  reads ~/.bash_profile  (which usually sources ~/.bashrc)
Interactive Shell  →  reads ~/.bashrc directly
```

---

## .rc Configuration Files

> **"rc"** stands for **"run commands"** — a UNIX tradition.  
> These are plain text scripts the shell automatically executes on startup. Use them to set aliases, functions, environment variables, your prompt, and PATH.

### `.bashrc`
- Sourced by **bash** for every new **interactive non-login** shell
- Location: `~/.bashrc`
- Common uses:

```sh
alias ll='ls -la'            # define aliases
export EDITOR=vim            # set environment variables
export PATH="$HOME/.local/bin:$PATH"  # extend PATH
```

> ⚠️ **Not** read for login shells by default — that's why `.bash_profile` usually sources it.

### `.bash_profile`
- Sourced by **bash** for **login shells**
- Location: `~/.bash_profile`
- Best practice — keep it minimal and source `.bashrc`:

```sh
if [ -f ~/.bashrc ]; then
    source ~/.bashrc
fi
```

### `.zshrc`
- Sourced by **zsh** for every new **interactive shell** (login or non-login on macOS)
- Location: `~/.zshrc`
- On macOS, this is **the single place** to configure zsh
- Popular frameworks:

| Framework | URL |
|-----------|-----|
| Oh My Zsh | https://ohmyz.sh |
| Prezto | https://github.com/sorin-ionescu/prezto |
| Zinit | https://github.com/zdharma-continuum/zinit |

### `.zprofile`
- zsh equivalent of `.bash_profile`; sourced for **login shells before** `.zshrc`
- Use for things that should only be set once at login:

```sh
# e.g. Homebrew PATH on Apple Silicon
eval "$(/opt/homebrew/bin/brew shellenv)"
```

### `.profile`
- POSIX-compatible login config; read by `sh` and as a fallback by `bash`
- Safe for environment variables shared across multiple shells

### Sourcing Order (macOS zsh login shell)

```
/etc/zshenv → ~/.zshenv → /etc/zprofile → ~/.zprofile → /etc/zshrc → ~/.zshrc
```

### Applying Changes Without Restarting

```sh
source ~/.zshrc
# or shorthand:
. ~/.zshrc
```

---

## Environment Variables

Key-value pairs available to the shell and all programs it launches.

```sh
export VAR=value       # set and export
echo $VAR              # read a variable
printenv VAR           # read via printenv
env                    # list all exported variables
unset VAR              # remove a variable
```

### Important Built-in Variables

| Variable | Description | Example Value |
|----------|-------------|---------------|
| `$HOME` | Current user's home directory | `/Users/ankit` |
| `$USER` | Current username | `ankit` |
| `$SHELL` | Path to the default shell | `/bin/zsh` |
| `$PATH` | Colon-separated directories searched for commands | `/usr/bin:/bin:...` |
| `$PWD` | Current working directory | `/Users/ankit/projects` |
| `$OLDPWD` | Previous working directory (`cd -`) | — |
| `$EDITOR` | Default text editor | `vim`, `nano`, `code` |
| `$PS1` / `$PROMPT` | Shell prompt format string | — |
| `$?` | Exit status of the last command | `0` = success |
| `$$` | PID of the current shell process | `12345` |

---

## PATH — The Most Important Environment Variable

When you type a command, the shell searches each directory in `$PATH` **left-to-right** until it finds a match.

```sh
# View current PATH
echo $PATH
# /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

# Prepend a directory (higher priority — searched first)
export PATH="$HOME/.local/bin:$PATH"

# Append a directory (lower priority — searched last)
export PATH="$PATH:$HOME/.local/bin"

# Find where a command lives
which python3        # → /usr/bin/python3
type python3         # → python3 is /usr/bin/python3
```

> Put `export PATH=...` in your `.zshrc` / `.bashrc` to make it permanent.

---

## Aliases

Short names for longer commands. Define them in `.zshrc` / `.bashrc`.

```sh
# Syntax
alias name='command'

# Examples
alias ll='ls -la'
alias gs='git status'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
alias python='python3'

# List all defined aliases
alias

# Remove an alias (current session only)
unalias name
```

---

## Shell Functions

More powerful than aliases — they accept **arguments** (`$1`, `$2`, ...) and can contain logic.

```sh
# Define in ~/.zshrc or ~/.bashrc
mkcd() {
    mkdir -p "$1" && cd "$1"
}
# Usage:
mkcd my-new-folder
```

| | Aliases | Functions |
|---|---|---|
| Accept arguments | ❌ | ✅ |
| Contain logic / loops | ❌ | ✅ |
| Multiple commands | Limited | ✅ |

---

## stdin, stdout, stderr — Standard Streams

Every process has three default I/O streams:

| Stream | FD | Default | Description |
|--------|----|---------|-------------|
| `stdin` | `0` | Keyboard | Data fed **into** a program |
| `stdout` | `1` | Screen | Normal **output** from a program |
| `stderr` | `2` | Screen | **Error** messages |

### Redirection

```sh
command > file          # stdout → file (overwrite)
command >> file         # stdout → file (append)
command < file          # file   → stdin
command 2> file         # stderr → file
command 2>&1            # stderr → same place as stdout
command > file 2>&1     # both stdout and stderr → file
command &> file         # shorthand for above (bash/zsh)

# Discard output entirely
command > /dev/null
command > /dev/null 2>&1
```

---

## Pipes

The `|` (pipe) operator sends the **stdout** of one command as the **stdin** of the next, letting you chain commands into powerful one-liners.

```sh
ls -la | grep ".txt"              # list only .txt files
cat file.txt | sort | uniq        # sort and deduplicate lines
ps aux | grep python              # find running python processes
history | grep git | tail -10     # last 10 git-related commands
```

---

## Command Chaining Operators

| Operator | Behaviour | Example |
|----------|-----------|---------|
| `;` | Run sequentially regardless of result | `mkdir foo ; cd foo` |
| `&&` | Run next **only if previous succeeded** (exit 0) | `mkdir foo && cd foo` |
| `\|\|` | Run next **only if previous failed** (exit ≠ 0) | `cd foo \|\| echo "Not found"` |
| `&` | Run command in the **background** (non-blocking) | `sleep 10 &` |

---

## History

The shell saves every command you type to a history file.

| Shell | History File |
|-------|-------------|
| bash | `~/.bash_history` |
| zsh | `~/.zsh_history` |

```sh
history           # print numbered command history
history 20        # show last 20 commands
!!                # re-run the last command
!n                # re-run command number n
!git              # re-run the last command starting with "git"
# Ctrl+R          # interactive reverse search through history
```

### Recommended `.zshrc` History Settings

```sh
HISTSIZE=10000
SAVEHIST=10000
setopt HIST_IGNORE_DUPS    # don't record duplicates
setopt SHARE_HISTORY       # share history across all open terminals
```

---

## Useful Shell Built-in Commands

| Command | Description |
|---------|-------------|
| `cd` | Change directory (`cd ~`, `cd -`, `cd ..`) |
| `pwd` | Print working directory |
| `echo` | Print text or variable value |
| `read` | Read user input into a variable |
| `export` | Mark a variable for export to child processes |
| `source` | Execute a file in the current shell (`. file` is shorthand) |
| `exit` | Exit the shell (with optional exit code) |
| `type` | Show how a name is interpreted (alias, builtin, file) |
| `which` | Show the full path of a command |
| `man` | Show the manual page for a command |
| `help` | Show help for shell builtins (bash) |

---

## Quoting Rules

| Quote Style | Behaviour | Example |
|-------------|-----------|---------|
| `'single quotes'` | Everything **literal** — no expansion | `echo '$HOME'` → `$HOME` |
| `"double quotes"` | Variables and `$()` expand; spaces preserved | `echo "$HOME"` → `/Users/ankit` |
| `` `backticks` `` | Command substitution (legacy) | `` echo `date` `` |
| `$(...)` | Command substitution (**preferred**) | `echo $(date)` |

```sh
# Preferred modern style
FILES=$(ls *.txt)
echo "Found files: $(ls | wc -l)"
```

---

## Startup File Quick Reference

| Shell | Login Shell | Interactive (non-login) |
|-------|-------------|------------------------|
| `sh` | `/etc/profile`, `~/.profile` | — |
| `bash` | `~/.bash_profile` | `~/.bashrc` |
| `zsh` | `~/.zprofile` | `~/.zshrc` |
| `fish` | `config.fish` | `config.fish` (always) |

> **macOS tip:** Terminal.app and iTerm2 open **login shells** by default, so `~/.zshrc` is sourced through the login chain. You can put everything in `.zshrc`.

---

*Last updated: March 2026*
