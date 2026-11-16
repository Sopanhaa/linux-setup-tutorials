# Linux System Maintenance

**Supported systems:** Ubuntu, Debian, Linux Mint, Fedora, Arch Linux, and
openSUSE

**Difficulty:** Beginner

**Terminal required:** Yes

**Last reviewed:** Not yet reviewed on live systems.

**General guides:** [All guides](./README.md)

## Overview

Regular maintenance keeps a Linux system secure and healthy. This guide covers:

- installing updates
- removing unused software and package caches
- reading system logs
- checking disk health
- planning backups

It does not cover distribution upgrades or recovery from a broken boot. Those
are linked at the end.

> **Safety:** Maintenance commands can remove software or logs. Read the list of
> changes before confirming. Keep a backup before large cleanups.

## Requirements

- A supported Linux system.
- `sudo` access.
- An internet connection for updates.
- Free disk space for updates.
- A backup of important files.

## Installation

### Keep the system updated

Use the commands for your distribution.

**Ubuntu, Debian, Linux Mint:**

```bash
sudo apt update
sudo apt upgrade
```

**Fedora:**

```bash
sudo dnf upgrade --refresh
```

**Arch Linux:**

```bash
sudo pacman -Syu
```

**openSUSE:**

```bash
sudo zypper refresh
sudo zypper update
```

On openSUSE Tumbleweed, use `sudo zypper dup` instead of `zypper update`.

Restart after a kernel, driver, or core library update:

```bash
sudo reboot
```

Read the update output. If the package manager asks about replacing a
configuration file, review the difference before choosing. Do not blindly accept
every default.

### Remove unused software

Remove a program you no longer use:

```bash
sudo apt remove package-name
sudo dnf remove package-name
sudo pacman -Rns package-name
sudo zypper remove package-name
```

Replace `package-name` with the real package. Removing a package can also remove
programs that depend on it; read the confirmation list.

### Clean package caches

Package managers keep downloaded packages. To free space:

```bash
sudo apt clean
```

```bash
sudo dnf clean all
```

```bash
sudo pacman -Sc
```

```bash
sudo zypper clean
```

These commands remove cached package files, not installed programs. The next
update will download packages again if needed.

### List orphaned packages on Arch Linux

```bash
pacman -Qtdq
```

This lists packages installed as dependencies that are no longer required. It
only reads information. Before removing them, check that you do not need them as
optional dependencies:

```bash
sudo pacman -Rns $(pacman -Qtdq)
```

> **Warning:** The second command removes every listed package. If the list
> contains something you need, remove packages one at a time instead. Do not run
> this command if you do not understand the list.

## Read logs

### Errors from the current boot

```bash
journalctl -b -p err
```

- `-b` means the current boot.
- `-p err` shows error-level messages and above.
- Press `q` to quit the pager.

### Failed services

```bash
systemctl --failed
```

This lists services that failed to start. Read the service's log:

```bash
journalctl -u service-name -b
```

Replace `service-name` with the name from the failed list.

### Check log disk usage

```bash
journalctl --disk-usage
```

If the journal is very large, remove old entries:

```bash
sudo journalctl --vacuum-time=14d
```

This keeps the last 14 days. Adjust the number to your needs. Do not delete files
inside `/var/log/journal` by hand.

## Check disk health

### Check free space

```bash
df -h
```

Look for filesystems that are close to 100% full. A full root filesystem can
prevent logins and updates.

### Check the drive's SMART status

Install `smartmontools`:

```bash
sudo apt install smartmontools
```

```bash
sudo dnf install smartmontools
```

```bash
sudo pacman -S smartmontools
```

```bash
sudo zypper install smartmontools
```

List drives:

```bash
lsblk -o NAME,SIZE,MODEL
```

Then check the drive. Replace `/dev/sdX` with the correct device. NVMe drives
usually appear as `/dev/nvme0n1`, and SATA or USB drives usually appear as
`/dev/sda`, `/dev/sdb`, and so on.

```bash
sudo smartctl -a /dev/sdX
```

> **Device warning:** `/dev/sdX` is an example. Check `lsblk` first. Do not run
> disk commands against a device you cannot identify.

Look for `SMART overall-health self-assessment test result: PASSED` and read the
error counters. A failing drive can still boot; back up your data before running
repairs.

## Back up your data

Updates and hardware can fail. A backup is the only reliable way to recover
personal files.

- Follow [Backup and restore](../maintenance/backup-and-restore.md).
- Keep at least one backup on a separate drive.
- Test restoring a file before you need it.

## Configuration

### Choose a maintenance schedule

A simple schedule:

| When | Task |
|---|---|
| Weekly | Install updates and check free disk space. |
| Monthly | Remove unused packages and check failed services. |
| Quarterly | Test a file restore and check disk health. |
| Before major changes | Create a backup and a system snapshot. |

### Automatic updates

Most distributions offer automatic security updates:

- Ubuntu and Debian use `unattended-upgrades`.
- Fedora uses `dnf-automatic`.
- openSUSE uses its online update tools.
- Arch Linux does not update automatically by default. Schedule manual updates.

Read your distribution's official documentation before enabling automatic
updates. Automatic updates can restart services or require a reboot.

### Keep old kernels

Do not delete old kernels manually unless your distribution's tool says to.
Keep at least one older kernel so you can boot if a new kernel fails. Your package
manager usually removes old kernels during normal cleanup.

## Verification

After maintenance:

```bash
df -h
systemctl --failed
journalctl -b -p err
```

Confirm that:

- [ ] Free disk space is reasonable.
- [ ] No services are failed.
- [ ] There are no new critical errors.
- [ ] The system restarts normally.
- [ ] Your backup still runs.

## Troubleshooting

### The disk is full after cleaning

- Check large directories:
  ```bash
  sudo du -xh --max-depth=1 /var | sort -h
  ```
- Check for old snapshots, container images, or downloaded ISO files.
- Do not delete files in `/usr` or `/var/lib` by hand. Use the package manager or
  the application's own cleanup command.

### `journalctl` says the journal is corrupted

- Back up important data first.
- Check free disk space.
- Follow your distribution's journal recovery instructions.
- Do not delete the entire journal unless you need to recover a system that
  cannot boot.

### `smartctl` reports `Permission denied`

Run it with `sudo`:

```bash
sudo smartctl -a /dev/sdX
```

### A package update fails

- Read the exact error.
- Refresh the package list.
- Check free space.
- Do not delete package-manager lock files.
- See [Package managers](./package-managers.md#troubleshooting).

### An update removed a program I need

Install the program again from your distribution's repository. If a third-party
repository was disabled, re-enable it only if you trust the source. Restore
configuration files from a backup if they were removed.

## Uninstall or Restore

Maintenance commands do not have a single undo button.

- Reinstall a removed package with your package manager.
- Restore deleted personal files from a backup.
- Restore a system snapshot if a change broke the system.
- If you removed old kernels, install the current kernel package again if needed.

Do not try to reverse a cleanup by copying files from another system. Package
versions and library paths may not match.

## References

- [ArchWiki: System maintenance](https://wiki.archlinux.org/title/System_maintenance)
- [Ubuntu: Automatic updates](https://documentation.ubuntu.com/server/how-to/software/automatic-updates/)
- [Fedora: DNF automatic](https://docs.fedoraproject.org/en-US/quick-docs/autoupdates/)
- [Debian: Unattended upgrades](https://wiki.debian.org/UnattendedUpgrades)
- [openSUSE: System updates](https://doc.opensuse.org/)
- [smartmontools](https://www.smartmontools.org/)
- [Backup and restore](../maintenance/backup-and-restore.md)
