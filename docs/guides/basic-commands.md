# Basic Linux Commands

**Supported systems:** Ubuntu, Debian, Linux Mint, Fedora, Arch Linux, and
openSUSE

**Difficulty:** Beginner

**Terminal required:** Yes

**Last reviewed:** Not yet reviewed on live systems.

**General guides:** [All guides](./README.md)

## Overview

This guide introduces the commands you need to move around the filesystem, work
with files, check permissions, and inspect running processes.

Commands are case-sensitive. A command, its options, and its file names must be
typed exactly. Linux does not ask for confirmation before every destructive
action, so read each command before pressing `Enter`.

## Requirements

- A Linux system and a user account.
- A terminal. On most desktops, press `Ctrl` + `Alt` + `T`.
- A practice folder. Do not practice in your home folder or system directories.

> **Safety:** Commands such as `rm`, `mv`, and `dd` can delete or overwrite data.
> Never run a command you do not understand. Do not copy commands from the
> internet without reading them.

## Open a terminal

Use one of these methods:

- Press `Ctrl` + `Alt` + `T` on Ubuntu, Linux Mint, and many other desktops.
- Search for **Terminal** in the application menu.
- On a server, sign in over SSH or use the local console.

The prompt usually ends with `$` for a normal user or `#` for the root user. If
you see `#`, be extra careful: the shell is running with administrator
privileges.

## Navigate the filesystem

### Find your location

```bash
pwd
```

`pwd` prints the current directory. Your home folder is usually
`/home/your-username`.

### List files

```bash
ls
ls -l
ls -a
```

- `ls` lists names.
- `ls -l` shows permissions, owner, size, and modification time.
- `ls -a` includes hidden files that start with a dot.

### Change directory

```bash
cd Documents
cd ..
cd ~
cd /
```

- `cd Documents` enters a folder relative to your current location.
- `cd ..` goes up one level.
- `cd ~` goes to your home folder.
- `cd /` goes to the root of the filesystem.

Use `Tab` to complete file and folder names. It reduces typing errors.

## Work with files and folders

### Create

```bash
mkdir practice
touch practice/notes.txt
```

- `mkdir` creates a directory.
- `touch` creates an empty file or updates its timestamp.

### Copy and move

```bash
cp practice/notes.txt practice/notes-copy.txt
mv practice/notes-copy.txt practice/notes-renamed.txt
```

- `cp` copies a file.
- `mv` moves or renames a file.

To copy or move a directory, add `-r`:

```bash
cp -r practice practice-backup
```

`-r` means recursive: it includes the folder and everything inside it.

### Read a file

```bash
cat practice/notes.txt
less /etc/os-release
```

- `cat` prints the whole file.
- `less` opens a pager. Use the arrow keys to scroll and `q` to quit.

For a long file, prefer `less`. `cat` can flood the terminal.

### Search inside files

```bash
grep "Ubuntu" /etc/os-release
```

`grep` prints lines that contain the text. Put the search text in quotes if it
contains spaces.

### Find files

```bash
find ~/Documents -name "*.pdf"
```

This searches your Documents folder for PDF files. `find` can be slow on large
directories.

### Delete files

```bash
rm practice/notes.txt
```

> **Warning:** `rm` deletes files without moving them to a trash folder. There is
> no undo. Check the path before pressing `Enter`. Never run `rm -rf /` or a
> command that deletes a directory you did not create.

To remove an empty directory:

```bash
rmdir practice
```

To remove a directory and its contents:

```bash
rm -r practice
```

Use `rm -r` only when you are sure the directory contains nothing you need.

## Understand permissions

List a file with details:

```bash
ls -l practice
```

The permissions look like `-rw-r--r--`:

- the first character is the file type (`-` for a regular file, `d` for a
  directory)
- the next three characters are the owner's permissions
- the next three are the group's permissions
- the last three are everyone else's permissions

`r` means read, `w` means write, and `x` means execute. For a directory, `x`
means you can enter it.

### Change permissions

```bash
chmod +x script.sh
chmod 644 notes.txt
```

- `chmod +x script.sh` makes a script executable.
- `chmod 644 notes.txt` gives the owner read and write, and everyone else read
  only.

Do not make files world-writable (`777`) to solve a permission problem. Fix the
ownership or use the correct group instead.

### Change ownership

```bash
sudo chown your-username:your-username file.txt
```

`chown` changes the owner and group. `sudo` is required for files you do not own.
Changing ownership of system files can break the system. Use it only on your own
files or when following official instructions.

## Processes and system information

### See running processes

```bash
ps aux
top
```

- `ps aux` prints a snapshot of running processes.
- `top` shows a live view. Press `q` to quit.

In `top`, the `%CPU` and `%MEM` columns show which processes use the most
resources.

### Stop a process

Find the process ID (PID) in `ps` or `top`, then send a normal termination
request:

```bash
kill PID
```

Replace `PID` with the number. If the process does not stop, you can force it:

```bash
kill -9 PID
```

> **Warning:** `kill -9` stops a process immediately and does not let it save
> data. Try `kill PID` first. Do not kill processes you do not recognize,
> especially system services.

### Check disks, memory, and network

```bash
df -h
free -h
ip address
```

- `df -h` shows free space on filesystems.
- `free -h` shows memory and swap.
- `ip address` shows network interfaces and IP addresses.

These commands only read information.

## Get help

```bash
man ls
ls --help
```

- `man` opens the manual page. Scroll with the arrow keys and press `q` to quit.
- `--help` prints a short usage summary for many commands.

Manual pages are the most reliable reference for the version installed on your
system.

## Practice safely

Create a practice folder and try the commands:

```bash
mkdir -p ~/linux-practice
cd ~/linux-practice
pwd
touch example.txt
ls -l
cp example.txt example-copy.txt
mv example-copy.txt renamed.txt
cat example.txt
cd ..
rm -r ~/linux-practice
```

The last command deletes the practice folder. Confirm that the path is
`~/linux-practice` before running it.

## Verification

You are ready to move on when you can:

- [ ] Print your current directory with `pwd`.
- [ ] List hidden files with `ls -a`.
- [ ] Create, copy, rename, and delete a file in a practice folder.
- [ ] Explain the difference between `cp` and `mv`.
- [ ] Read a file with `less`.
- [ ] Find a process with `ps aux` and stop it with `kill`.
- [ ] Open a manual page with `man`.

## Troubleshooting

### `command not found`

- Check the spelling. Linux commands are case-sensitive.
- The program may not be installed. Use your package manager to install it.
- The command may be in a directory that is not in your `PATH`.

### `Permission denied`

- You may not have permission to read, write, or execute the file.
- Check with `ls -l`.
- Use `sudo` only when the task requires administrator access. Do not use `sudo`
  to work around a problem with your own files.

### `No such file or directory`

- Check the path with `pwd` and `ls`.
- Use `Tab` completion.
- File and folder names are case-sensitive.

### A command is stuck

Press `Ctrl` + `C` to interrupt it. This sends an interrupt signal and usually
returns you to the prompt. It may not stop a process that is ignoring signals.

## Uninstall or Restore

This guide does not install software. To clean up practice files:

```bash
rm -r ~/linux-practice
```

If you changed permissions and need to restore them, use `chmod` with the
original mode. If you do not know the original mode, restore the file from a
backup. See [Backup and restore](../maintenance/backup-and-restore.md).

## References

- [Ubuntu command-line documentation](https://ubuntu.com/tutorials/command-line-for-beginners)
- [Debian reference](https://www.debian.org/doc/manuals/debian-reference/)
- [ArchWiki: Command-line shell](https://wiki.archlinux.org/title/Command-line_shell)
- [GNU Coreutils manual](https://www.gnu.org/software/coreutils/manual/)
- [Linux man-pages project](https://www.kernel.org/doc/man-pages/)
