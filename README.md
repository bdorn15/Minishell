# 🐚 minishell

> *As beautiful as a shell* — a custom Unix shell built from scratch in C, recreating the core behavior of bash.

![C](https://img.shields.io/badge/language-C-blue?style=flat-square)
![42 School](https://img.shields.io/badge/school-42-black?style=flat-square)
![Norm](https://img.shields.io/badge/code%20style-norminette-green?style=flat-square)

---

## 📖 About

**minishell** is a project from 42 School that challenges you to implement a functional Unix shell from scratch. It deepens understanding of processes, file descriptors, signals, and how a shell actually works under the hood.

The shell supports interactive use with a prompt, command history, pipes, redirections, environment variable expansion, and a set of built-in commands — closely mimicking the behavior of `bash`.

---

## ✨ Features

### Core Shell Behavior
- **Interactive prompt** — displays a prompt and waits for user input
- **Command history** — navigate previous commands with arrow keys (via `readline`)
- **Executable lookup** — resolves commands using `PATH`, relative, or absolute paths

### Quoting & Parsing
| Token | Behavior |
|---|---|
| `'single quotes'` | Prevents all interpretation of meta-characters |
| `"double quotes"` | Prevents all interpretation except `$` (dollar sign) |

### Redirections
| Operator | Description |
|---|---|
| `<` | Redirect stdin from a file |
| `>` | Redirect stdout to a file (overwrite) |
| `>>` | Redirect stdout to a file (append) |
| `<<` | Here-doc: read input until a delimiter line is seen |

### Pipes
- `cmd1 \| cmd2 \| cmd3` — chains commands, connecting stdout of each to stdin of the next

### Environment Variables
- `$VAR` expands to the value of `VAR`
- `$?` expands to the exit status of the last foreground pipeline

### Signal Handling
| Signal | Interactive behavior |
|---|---|
| `Ctrl-C` | Displays a new prompt on a new line |
| `Ctrl-D` | Exits the shell |
| `Ctrl-\` | Does nothing |

> ⚠️ A single global variable is used exclusively to store the received signal number — no structs or additional data in global scope.

---

## 🔧 Built-in Commands

| Command | Description |
|---|---|
| `echo [-n]` | Print text; `-n` suppresses trailing newline |
| `cd [path]` | Change directory (relative or absolute path) |
| `pwd` | Print current working directory |
| `export [var=value]` | Set or export environment variables |
| `unset [var]` | Remove environment variables |
| `env` | Print all environment variables |
| `exit` | Exit the shell |

---

## 🚀 Getting Started

### Requirements
- GCC or Clang
- GNU Make
- `readline` and `history` libraries — install with your package manager:

```bash
# Debian / Ubuntu
sudo apt-get install libreadline-dev

# macOS (Homebrew)
brew install readline
```

### Build

```bash
git clone https://github.com/yourusername/minishell.git
cd minishell
make
```

### Run

```bash
./minishell
```

### Makefile Targets

```bash
make        # Build the binary
make clean  # Remove object files
make fclean # Remove object files and binary
make re     # fclean + build
```

---

## 🔍 Implementation Notes

- **Parsing** is done in multiple passes: lexing (tokenizing the raw input), then parsing (building a command table / AST) with proper handling of quotes and expansions.
- **Execution** forks child processes for external commands; built-ins run in the current process (or in a subshell when part of a pipeline).
- **Pipes** are implemented with `pipe()`, `fork()`, and `dup2()` to wire file descriptors between commands.
- **Here-doc (`<<`)** reads from a temporary pipe until the delimiter is matched, without updating history.
- **Memory**: All heap allocations are tracked and freed. Note that `readline()` itself may produce leaks, but those are out of scope per the project spec.

---

## 📚 Allowed External Functions

`readline`, `rl_clear_history`, `rl_on_new_line`, `rl_replace_line`, `rl_redisplay`, `add_history`, `printf`, `malloc`, `free`, `write`, `access`, `open`, `read`, `close`, `fork`, `wait`, `waitpid`, `signal`, `sigaction`, `sigemptyset`, `sigaddset`, `kill`, `exit`, `getcwd`, `chdir`, `stat`, `lstat`, `fstat`, `unlink`, `execve`, `dup`, `dup2`, `pipe`, `opendir`, `readdir`, `closedir`, `strerror`, `perror`, `isatty`, `ttyname`, `ttyslot`, `ioctl`, `getenv`, `tcsetattr`, `tcgetattr`, `tgetent`, `tgetflag`, `tgetnum`, `tgetstr`, `tgoto`, `tputs`

---

## 📝 License

This project is part of the 42 School curriculum. Feel free to use it as a reference, but please don't submit it as your own work.

---

*Tested against bash behavior. When in doubt, bash is the reference.*
