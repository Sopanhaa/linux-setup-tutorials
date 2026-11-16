# Backup and Restore

**Supported systems:** Ubuntu, Debian, Linux Mint, Fedora, Arch Linux, and
openSUSE

**Difficulty:** Beginner

**Terminal required:** For the `rsync` method, yes. The graphical tools can be
used without a terminal.

**Last reviewed:** Not yet reviewed on live systems. Commands follow the
documented options of each tool; verify paths on your own machine.

## Overview

This tutorial sets up two kinds of protection:

1. **A file backup** of your home folder using `rsync`, Déjà Dup, or your
   distribution's backup tool.
2. **A system snapshot** with Timeshift, so you can return to a working system
   after a bad update or driver change.

A file backup protects documents, photos, and code. A system snapshot protects
the operating system and installed software. You usually want both, stored in
different places.

This tutorial does **not** cover full disk imaging, encrypted backups, or
enterprise backup servers.

## Requirements

Before you start:

- A separate USB drive or external disk with enough free space. A good starting
  point is at least the size of the data you want to protect.
- A backup drive that can be formatted. **Formatting erases everything on the
  drive.**
- Your user password for `sudo` commands.
- Enough time to copy your files and test a restore.
- A second copy location if the data is important. Follow the 3-2-1 rule: three
  copies, on two kinds of media, with one copy off-site.

> **Warning:** Do not store the only copy of a file on the same disk as the
> original. If the disk fails, you lose both copies.

### Choose a filesystem for the backup drive

| Filesystem | Good for | Limitation |
|---|---|---|
| ext4 | Linux-only backups | Windows cannot read it without extra software |
| exFAT | Moving files between Linux, Windows, and macOS | Does not preserve Linux ownership and permissions |
| NTFS | Sharing with Windows | Does not fully preserve Linux ownership and permissions |

Use ext4 when you want a faithful Linux backup. Use exFAT or NTFS when you also
need to read the drive on Windows, and understand that permissions and some
metadata may not be preserved.

## Installation

### Step 1: Identify the backup drive

Connect the drive, then run:

```bash
lsblk -o NAME,SIZE,FSTYPE,LABEL,MOUNTPOINT
```

- `lsblk` lists block devices such as disks and partitions.
- `NAME` is the device name, `SIZE` is its capacity, `FSTYPE` is the filesystem,
  `LABEL` is the volume name, and `MOUNTPOINT` is where it is mounted.

This command only reads information. Confirm the drive by its size and label.
Do not format or repartition a device unless you are certain which one it is.

If the drive is not mounted, open it in your file manager. It usually appears
under `/media/$USER/LABEL`, where `$USER` is your username and `LABEL` is the
drive's label.

### Step 2: Install the backup tools

Install `rsync` and, if you want graphical tools, Déjà Dup and Timeshift.

**Ubuntu, Debian, Linux Mint:**

```bash
sudo apt update
sudo apt install rsync deja-dup timeshift
```

**Fedora:**

```bash
sudo dnf install rsync deja-dup timeshift
```

**Arch Linux:**

```bash
sudo pacman -S rsync deja-dup timeshift
```

**openSUSE:**

```bash
sudo zypper install rsync deja-dup timeshift
```

`sudo` runs the install command with administrator privileges. `rsync` copies
files efficiently. Déjà Dup is a graphical backup tool. Timeshift creates system
snapshots.

If a package is not available in your enabled repositories, check your
distribution's official documentation for the correct package source. Do not
install an unofficial build from a random website.

### Step 3: Create the backup folder

Choose a folder on the backup drive. For example:

```bash
mkdir -p "/media/$USER/BACKUP/home"
```

- `mkdir` creates a directory.
- `-p` creates parent directories if needed and does not fail if the folder
  already exists.
- The quotes keep the path together if your username or label contains spaces.

Replace `BACKUP` with the label of your drive.

### Step 4: Test the copy with `rsync`

Always test with `--dry-run` first. This shows what would happen without copying
anything:

```bash
rsync -av --dry-run "/home/$USER/" "/media/$USER/BACKUP/home/"
```

- `-a` preserves permissions, timestamps, and symbolic links. It also copies
  directories recursively.
- `-v` prints the files being processed.
- `--dry-run` makes no changes.

Check the list. When it looks correct, run the same command without `--dry-run`:

```bash
rsync -av "/home/$USER/" "/media/$USER/BACKUP/home/"
```

> **Trailing slashes matter.** `/home/$USER/` with a trailing slash copies the
> *contents* of your home folder. `/home/$USER` without the slash copies the
> folder itself. Use the form shown here for this layout.

A first backup can take a long time. You can stop it with `Ctrl` + `C` and run it
again later. `rsync` resumes by comparing files.

### Step 5: Optional — keep the backup in sync

After the first full copy, you can update the backup with:

```bash
rsync -av --delete "/home/$USER/" "/media/$USER/BACKUP/home/"
```

> **Warning:** `--delete` removes files from the backup when they no longer exist
> in the source. That is useful for a mirror, but dangerous if the source is
> incomplete or mounted incorrectly. Run the command with `--dry-run` first and
> read the deletions carefully.

If you are not comfortable with `--delete`, omit it. The backup will keep old
files, which uses more space but is safer.

### Step 6: Optional — use Déjà Dup

Déjà Dup provides a graphical backup with scheduling and encryption.

1. Open **Déjà Dup** or **Backups** from the application menu.
2. Choose **Create my first backup** or open **Preferences**.
3. Select the folders to back up. The home folder is a good default.
4. Choose the backup drive or a folder on it.
5. Turn on **Automatic backups** and choose a schedule, such as weekly.
6. Optionally enable encryption. Store the password safely; an encrypted backup
   cannot be restored without it.
7. Run the first backup and wait for it to finish.

Keep the backup drive connected for scheduled backups, or plug it in when Déjà
Dup asks for it.

### Step 7: System snapshots with Timeshift

Timeshift snapshots the system, not your personal files. It is useful before a
driver update, a distribution upgrade, or a major configuration change.

1. Open **Timeshift**.
2. Choose **RSYNC** or **BTRFS** based on your filesystem and the tool's
   recommendation.
3. Select the disk where snapshots will be stored. An external drive is better
   than the system disk.
4. Choose a schedule, or create snapshots manually before big changes.
5. Create a first snapshot and wait for it to finish.

> **Do not rely on Timeshift for documents or photos.** By default, it excludes
> home-directory files. Use a file backup for those.

## Configuration

### Set a backup schedule

- **Déjà Dup:** enable automatic backups and choose daily or weekly.
- **Timeshift:** choose a schedule in **Settings → Schedule**. Keep at least one
  older snapshot so you can go back further than the most recent change.
- **Manual `rsync`:** add the command to a script or run it on a regular day.
  Test the script before trusting it.

### Exclude temporary files

For a home backup, you can exclude caches and trash to save space:

```bash
rsync -av --delete \
  --exclude='.cache/' \
  --exclude='.local/share/Trash/' \
  "/home/$USER/" "/media/$USER/BACKUP/home/"
```

The backslash `\` at the end of a line continues the command on the next line.
Type the command as one logical command.

### Keep one copy offline

Disconnect or unmount the backup drive after the backup finishes:

```bash
udisksctl unmount -b /dev/sdXN
```

Replace `/dev/sdXN` with the drive's actual partition. You can also use the eject
button in your file manager. Unmounting is safer than unplugging a drive that is
still writing.

## Verification

### Check that the backup contains files

```bash
du -sh "/home/$USER"
du -sh "/media/$USER/BACKUP/home"
```

`du -sh` shows the total size of a directory. The sizes may differ because of
exclusions, filesystem overhead, and deleted files, but the backup should not be
empty.

### Compare a second run

Run the dry run again:

```bash
rsync -av --dry-run "/home/$USER/" "/media/$USER/BACKUP/home/"
```

If the backup is current, the output should show few or no file transfers.

### Restore one test file

This is the most important check. Copy one file back to a temporary folder:

```bash
mkdir -p /tmp/restore-test
rsync -av "/media/$USER/BACKUP/home/Documents/example.txt" /tmp/restore-test/
diff "/home/$USER/Documents/example.txt" /tmp/restore-test/example.txt
```

- `mkdir -p` creates the test folder.
- `rsync` copies the file.
- `diff` compares the two files. No output means the contents match.

Replace `Documents/example.txt` with a real file. Remove the test folder when
finished:

```bash
rm -r /tmp/restore-test
```

`rm -r` deletes a directory and its contents. Check the path before running it.

## Troubleshooting

### `Permission denied`

- Confirm that the backup drive is mounted and writable by your user.
- If the drive is formatted with a Linux filesystem, check ownership with
  `ls -ld /media/$USER/BACKUP`.
- Do not use `sudo rsync` unless you understand the ownership changes it can
  cause. Backups made with `sudo` may be owned by `root`.

### The drive is read-only

- Check the drive's physical write-protect switch, if it has one.
- Unmount and reconnect the drive.
- A failing drive or a dirty NTFS filesystem may be mounted read-only. Back up
  important data before attempting repairs.

### `No space left on device`

- Check free space with `df -h`.
- Remove old snapshots or old backup copies you no longer need.
- Use `--delete` only after confirming the source is complete.
- Consider excluding caches, virtual machine images, and large downloads.

### Timeshift cannot find a suitable partition

- Timeshift needs a filesystem it supports. Check its official documentation for
  Btrfs and external-drive requirements.
- Do not reformat your system disk just to make Timeshift work.
- Use a file backup instead if snapshots are not available.

### The backup is much smaller than expected

- Check for exclusions in the command or GUI settings.
- Confirm that the correct user account is being backed up.
- Check whether the backup stopped early because the drive was full or
  disconnected.

## Uninstall or Restore

### Restore a single file or folder

First test the restore with `--dry-run`:

```bash
rsync -av --dry-run "/media/$USER/BACKUP/home/Documents/" "/home/$USER/Documents/"
```

Then run it without `--dry-run`:

```bash
rsync -av "/media/$USER/BACKUP/home/Documents/" "/home/$USER/Documents/"
```

> **Warning:** Restoring over an existing folder can overwrite newer files. Back
> up the current version first, or restore to a different folder and copy only
> the files you need.

### Restore the whole home folder

This overwrites files with the same names in your home folder:

```bash
rsync -av "/media/$USER/BACKUP/home/" "/home/$USER/"
```

Sign out before restoring the whole home folder if possible. Do not run this
command while the backup drive is unmounted or while the path is wrong.

### Restore a Timeshift snapshot

1. Open Timeshift.
2. Select the snapshot you want.
3. Choose **Restore** and review the summary.
4. Restart when prompted.

If the system cannot boot, start a live USB, install and open Timeshift, select
the snapshot location, and restore from there. Follow Timeshift's on-screen
instructions.

### Remove the backup tools

**Ubuntu, Debian, Linux Mint:**

```bash
sudo apt remove deja-dup timeshift
```

**Fedora:**

```bash
sudo dnf remove deja-dup timeshift
```

**Arch Linux:**

```bash
sudo pacman -Rns deja-dup timeshift
```

**openSUSE:**

```bash
sudo zypper remove deja-dup timeshift
```

Removing the tools does not delete your backup files. Delete those only after
you have another working backup.

## References

- [`rsync` manual](https://download.samba.org/pub/rsync/rsync.1)
- [Déjà Dup documentation](https://wiki.gnome.org/Apps/DejaDup)
- [Timeshift project](https://github.com/linuxmint/timeshift)
- [ArchWiki: System backup](https://wiki.archlinux.org/title/System_backup)
- [Ubuntu: Desktop how-to guides](https://documentation.ubuntu.com/desktop/en/latest/how-to/)
- [Fedora: Quick docs](https://docs.fedoraproject.org/en-US/quick-docs/)
- [openSUSE: Documentation](https://doc.opensuse.org/)

[Back to maintenance](./README.md) · [Back to documentation home](../README.md)
