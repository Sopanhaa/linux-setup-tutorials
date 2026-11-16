# Troubleshooting — Ubuntu

**Supported version:** Ubuntu Desktop 26.04 LTS

**Difficulty:** Beginner to intermediate

**Terminal required:** Yes for most checks.

**Last reviewed:** Not yet reviewed on live systems.

**Learning path:** ← [Desktop Setup](./05-desktop-setup.md) · [Ubuntu overview](./README.md)

## Overview

This page covers common Ubuntu installation and configuration problems. Start
with the read-only checks, then apply the smallest fix that matches the error.

For problems that affect any distribution, see the
[general troubleshooting hub](../troubleshooting/README.md).

> **Before you change anything:** back up important files. If Ubuntu cannot boot,
> use a live USB to copy your files before repairing the system.

## Requirements

- Access to the Ubuntu desktop or a text console.
- A live Ubuntu USB drive for boot or disk problems.
- Your user password for `sudo`.
- The exact error message and the time the problem started.
- A backup of important files, when the system still boots.

## Installation

This page does not install software. The steps below diagnose and repair an
existing installation.

### Step 1: Collect basic information

Open a terminal with `Ctrl` + `Alt` + `T` and run:

```bash
cat /etc/os-release
uname -r
df -h
free -h
```

- `cat /etc/os-release` shows the Ubuntu version.
- `uname -r` shows the running kernel.
- `df -h` shows free disk space.
- `free -h` shows memory and swap.

Write down the Ubuntu version and kernel version before asking for help.

### Step 2: Check failed services and recent errors

```bash
systemctl --failed
journalctl -b -p err
```

- `systemctl --failed` lists services that did not start.
- `journalctl -b -p err` shows error-level messages from the current boot.

Press `q` to quit the log pager. Read the full message around the error; the
last line is not always the cause.

### Step 3: Check the hardware

```bash
lspci -k | grep -A 3 -E 'VGA|3D|Network'
```

This lists graphics and network devices with their kernel drivers. If a device
shows no `Kernel driver in use` line, the driver may be missing.

## Common problems and fixes

### The installation USB does not boot

1. Open the one-time boot menu and select the USB entry.
2. Try another USB port.
3. Recreate the USB with Rufus or Ventoy.
4. Verify the ISO checksum.
5. Check UEFI versus legacy boot mode.

See [Create a bootable USB drive](../guides/bootable-usb.md#troubleshooting).

### Ubuntu boots to a black screen

- Wait a minute. Some graphics drivers take time to initialize.
- Disconnect extra monitors.
- At the GRUB menu, choose **Advanced options for Ubuntu** and boot an older
  kernel.
- Choose **Recovery mode** and use the available repair options.
- If the problem started after a driver change, remove the driver from recovery
  mode or a text console.

See [Graphics drivers](../guides/graphics-drivers.md#troubleshooting).

### No Wi-Fi or no network

Check the device and connection state:

```bash
nmcli device status
nmcli radio wifi
rfkill list
ip address
```

- `nmcli device status` shows NetworkManager devices.
- `nmcli radio wifi` shows whether Wi-Fi is enabled.
- `rfkill list` shows software and hardware radio blocks.
- `ip address` shows network interfaces and addresses.

If Wi-Fi is blocked by software:

```bash
nmcli radio wifi on
```

If a hardware block is listed, check the laptop's Wi-Fi switch or firmware
setting. If the adapter is not detected at all, use Ethernet or USB tethering to
install updates and drivers.

### Wi-Fi keeps disconnecting

1. Check whether the problem affects only one network.
2. Restart NetworkManager:
   ```bash
   sudo systemctl restart NetworkManager
   ```
   This briefly disconnects all network connections.
3. Check the log:
   ```bash
   journalctl -u NetworkManager -b
   ```
4. Update the system and firmware if updates are available.
5. For a specific adapter, follow the official Ubuntu or vendor documentation.

### Sound does not work

Check the output device:

```bash
pactl info
```

Open **Settings → Sound** and confirm the correct output device is selected. If
the device is missing, check `lspci -k` for the audio device and its driver.

For a terminal mixer, run:

```bash
alsamixer
```

Use the arrow keys to select a channel and `M` to unmute it. Press `Esc` to quit.
Do not raise every slider to maximum; start at a moderate level.

### The package manager is locked

If you see `Could not get lock`, another update or software installation is
running. Wait for it to finish, or restart the computer. Do not delete the lock
file.

### Updates fail after a release upgrade

- Check the exact repository URL in the error.
- Confirm the Ubuntu version in `/etc/os-release` matches your sources.
- Disable third-party repositories that do not support the new release.
- Do not mix packages from different Ubuntu releases.

### The disk is full

Check the largest space users:

```bash
df -h
sudo du -xh --max-depth=1 /var | sort -h
```

- `df -h` shows free space.
- `du` estimates directory sizes. `-x` stays on one filesystem, `-h` uses
  human-readable units, and `--max-depth=1` limits the output.

Safe cleanup options:

```bash
sudo apt clean
sudo journalctl --vacuum-time=7d
```

- `apt clean` removes downloaded package files from the cache.
- `journalctl --vacuum-time=7d` removes journal logs older than seven days.

Do not delete files in `/var` or `/usr` by hand unless you know exactly what they
are.

### Suspend or resume fails

- Note whether the problem happens on battery, on AC power, or both.
- Disconnect USB devices and try again.
- Check the log after the next failed resume:
  ```bash
  journalctl -b -1 -p err
  ```
- Update the kernel and firmware if updates are available.
- For NVIDIA systems, check the driver and suspend documentation.

### Windows is missing from the boot menu

```bash
sudo update-grub
```

`update-grub` rebuilds the GRUB menu. Read the output for a Windows entry. If
Windows still does not appear, do not run random repair scripts. Read
[Dual-booting Linux and Windows](../guides/dual-boot.md) and back up your files
first.

> **BitLocker warning:** Changing boot order or firmware settings can trigger a
> BitLocker recovery prompt. Have the recovery key before making changes.

### An application does not start

1. Open a terminal and run the application from there to see the error message.
2. Check free disk space.
3. Update the application and the system.
4. Remove recently installed extensions or themes if the problem started after a
   desktop change.
5. Reinstall only the affected package:
   ```bash
   sudo apt install --reinstall package-name
   ```
   Replace `package-name` with the actual package. This does not remove your
   personal files.

## Verification

After a fix, confirm that it survives a reboot:

1. Restart the computer.
2. Repeat the check that failed.
3. Run `systemctl --failed` again.
4. Confirm that networking, sound, display, and your main applications work.
5. If you changed a driver, check the driver in use with `lspci -k`.

## Troubleshooting the troubleshooting

If a fix makes things worse:

- Stop and write down what you changed.
- Undo the last change before trying another fix.
- Do not apply several fixes at once; you will not know which one worked.
- Ask for help with the exact error, Ubuntu version, kernel version, and the
  steps you already tried.

## Uninstall or Restore

### Undo a package change

Remove a package you added:

```bash
sudo apt remove package-name
```

Remove its system-wide configuration as well:

```bash
sudo apt purge package-name
```

`purge` removes configuration files. It does not remove your personal files.

### Undo a driver change

Open **Software & Updates → Additional Drivers** and select the previous driver.
If the desktop does not start, use recovery mode or a text console.

### Restore the system

If you created a Timeshift snapshot before the change:

1. Open Timeshift.
2. Select the snapshot.
3. Choose **Restore** and review the summary.
4. Restart when prompted.

If Ubuntu cannot boot, use a live USB and follow
[Backup and restore](../maintenance/backup-and-restore.md#restore-a-timeshift-snapshot).

### Reinstall Ubuntu

Reinstalling is a last resort. Back up your files first. The Ubuntu installer can
replace the system, but it can also erase the disk if you choose the wrong
option. Read [Installation](./03-installation.md) again before starting.

## References

- [Ubuntu Desktop documentation](https://ubuntu.com/desktop/docs/)
- [Ubuntu community help](https://help.ubuntu.com/community/)
- [Ubuntu Discourse](https://discourse.ubuntu.com/)
- [Ask Ubuntu](https://askubuntu.com/)
- [Ubuntu release notes](https://documentation.ubuntu.com/release-notes/)
- [ArchWiki: General troubleshooting](https://wiki.archlinux.org/title/General_troubleshooting)
- [General troubleshooting hub](../troubleshooting/README.md)
