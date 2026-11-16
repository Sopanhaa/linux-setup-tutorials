# Troubleshooting

This page covers common installation and configuration problems. It starts with
safe checks and points to distribution-specific pages for deeper help.

> **Before you change anything:** back up important files if the computer still
> boots. If it does not boot, use a live USB to copy your files before repairing
> the system.

## First checks

Work from the outside in. These steps are read-only unless stated otherwise.

1. **Read the exact error message.** Write it down or take a photo.
2. **Check free disk space.** A full disk can stop updates, logins, and
   installers.
3. **Check the installation media.** Verify the ISO checksum and recreate the
   USB if the installer behaves unpredictably.
4. **Check the network.** Confirm that the computer has an IP address and can
   reach the internet.
5. **Check recent changes.** New drivers, kernel updates, extensions, or
   package removals often explain a sudden problem.
6. **Check the system logs.** They usually name the failing device or service.

### Read-only diagnostic commands

Open a terminal and run:

```bash
df -h
free -h
ip address
```

- `df -h` shows free space on each filesystem.
- `free -h` shows memory and swap use.
- `ip address` shows network interfaces and addresses.

To list failed system services:

```bash
systemctl --failed
```

To read errors from the current boot:

```bash
journalctl -b -p err
```

`journalctl` opens a pager. Use the arrow keys to scroll and press `q` to quit.

## Common problems

### The USB drive does not appear in the boot menu

- Reconnect the drive and try another USB port.
- Open the one-time boot menu, not only the firmware settings screen.
- Confirm the USB was written with Rufus, Ventoy, or your distribution's
  recommended tool.
- Check whether the computer boots in UEFI or legacy mode and whether the USB
  supports that mode.

See [Create a bootable USB drive](../guides/bootable-usb.md#troubleshooting).

### The installer starts, but no disk is detected

- Some laptops ship with Intel RST/RAID mode enabled. Changing it to AHCI may be
  required, but it can break an existing Windows installation. Follow the
  computer manufacturer's instructions and back up first.
- Check the drive in the firmware settings.
- Try the distribution's latest release or a newer kernel.

### Wi-Fi does not work after installation

- Check whether the adapter is detected:
  ```bash
  lspci -k | grep -A 3 -i network
  ```
- Check whether Wi-Fi is blocked:
  ```bash
  rfkill list
  ```
- Connect with an Ethernet cable or USB tethering to install updates and
  drivers.
- For Broadcom or other unusual adapters, follow your distribution's official
  hardware documentation.

### The screen is black or the desktop does not start

- Try another display cable or port.
- At the login screen, check whether Wayland or X11 is selected. Some NVIDIA
  configurations work better with one of them.
- Boot a live USB and check that the display works there. This separates a
  hardware problem from an installed-system problem.
- If you recently changed a graphics driver, boot an older kernel or a recovery
  mode and remove the driver.

See [Graphics drivers](../guides/graphics-drivers.md#troubleshooting).

### Updates fail

- Check the error for a missing repository or a network failure.
- Refresh the package list first. For Ubuntu, Debian, and Linux Mint:
  ```bash
  sudo apt update
  ```
- Repair interrupted package operations:
  ```bash
  sudo dpkg --configure -a
  ```
  This finishes package installations that were interrupted. Run it only when
  the package manager reports an interrupted operation.
- Do not delete package-manager lock files to "fix" a busy error. Wait for the
  running update to finish, or restart the computer.

See [Package managers](../guides/package-managers.md#troubleshooting).

### The computer does not boot after installation

- Check the boot order in the firmware settings.
- For a dual-boot system, check whether Windows Fast Startup or BitLocker is
  involved before changing boot settings.
- Boot a live USB and back up your files before attempting a repair.
- Use your distribution's official recovery or chroot instructions. Do not run
  random boot-repair scripts from the internet.

See [Dual-booting Linux and Windows](../guides/dual-boot.md) and
[Backup and restore](../maintenance/backup-and-restore.md).

### A desktop extension breaks the desktop

- Log out and log back in.
- Open the Extensions or Extension Manager application and disable the newest
  extension.
- If the desktop cannot load, switch to a text console with `Ctrl` + `Alt` + `F3`,
  sign in, and disable the extension from the command line if you know its
  identifier. Otherwise, ask your distribution's community for help.

## Before you reinstall

Reinstalling is not a troubleshooting step. Try these first:

- [ ] Back up important files.
- [ ] Read the error message and the system logs.
- [ ] Check disk space and memory.
- [ ] Remove changes made just before the problem started.
- [ ] Test the hardware with a live USB.
- [ ] Search your distribution's official documentation and bug tracker.
- [ ] Ask in an official community channel with the exact error and your
  distribution version.

## Where to get help

- [Ubuntu Discourse](https://discourse.ubuntu.com/) and
  [Ask Ubuntu](https://askubuntu.com/)
- [Fedora Discussion](https://discussion.fedoraproject.org/)
- [Arch Linux forums](https://bbs.archlinux.org/) and
  [ArchWiki](https://wiki.archlinux.org/)
- [Debian user mailing lists](https://www.debian.org/MailingLists/)
- [Linux Mint forums](https://forums.linuxmint.com/)
- [openSUSE forums](https://forums.opensuse.org/)

When asking for help, include:

- distribution and version
- desktop environment
- exact error message
- what you changed before the problem started
- whether a live USB works

## References

- [Ubuntu documentation](https://documentation.ubuntu.com/)
- [Fedora documentation](https://docs.fedoraproject.org/)
- [ArchWiki](https://wiki.archlinux.org/)
- [Debian documentation](https://www.debian.org/doc/)
- [openSUSE documentation](https://doc.opensuse.org/)

[Back to documentation home](../README.md) · [Back to project README](../../README.md)
