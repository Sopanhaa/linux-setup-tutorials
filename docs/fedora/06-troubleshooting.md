# Troubleshooting — Fedora

**Supported version:** Fedora Workstation 45

**Difficulty:** Beginner to intermediate

**Terminal required:** Yes for most checks.

**Last reviewed:** Not yet reviewed on live systems.

**Learning path:** ← [Desktop Setup](./05-desktop-setup.md) · [Fedora overview](./README.md)

## Overview

This page covers common Fedora Workstation installation and configuration
problems. Start with the read-only checks, then apply the smallest fix that
matches the error.

For problems that affect any distribution, see the
[general troubleshooting hub](../troubleshooting/README.md).

> **Before you change anything:** back up important files. If Fedora cannot boot,
> use a live USB to copy your files before repairing the system.

## Requirements

- Access to the Fedora desktop or a text console.
- A Fedora Workstation Live USB for boot or disk problems.
- Your user password for `sudo`.
- The exact error message and the time the problem started.
- A backup of important files, when the system still boots.

## Installation

This page does not install software. The steps below diagnose and repair an
existing installation.

### Step 1: Collect basic information

Open a terminal and run:

```bash
cat /etc/os-release
uname -r
df -h
free -h
```

- `cat /etc/os-release` shows the Fedora version.
- `uname -r` shows the running kernel.
- `df -h` shows free disk space.
- `free -h` shows memory and swap.

Write down the Fedora version and kernel version before asking for help.

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
3. Recreate the USB with Fedora Media Writer.
4. Verify the ISO checksum and signature.
5. Check UEFI versus legacy boot mode.

See [Create a bootable USB drive](../guides/bootable-usb.md#troubleshooting).

### The installer cannot find a disk

- Some laptops use Intel RST/RAID mode. Changing it to AHCI may be required, but
  it can break an existing Windows installation. Back up and follow the laptop
  manufacturer's instructions first.
- Check that the disk is visible in the firmware settings.
- If Windows uses BitLocker, do not delete its partition from the installer.
  Boot Windows and shrink or decrypt the volume properly first.
- Check the Fedora 45 release notes for known storage issues.

### Fedora boots to a black screen

- Wait a minute. Some graphics drivers take time to initialize.
- Disconnect extra monitors.
- At the GRUB menu, choose an older kernel under **Advanced options**.
- Use recovery mode if it is available.
- If the problem started after a driver change, remove the driver from a text
  console or recovery mode.

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
install updates and firmware.

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
5. For a specific adapter, follow the Fedora wireless documentation or the
   vendor's Linux instructions.

### No sound

Fedora Workstation uses PipeWire. Check the audio graph:

```bash
wpctl status
pactl info
```

- `wpctl status` shows PipeWire sinks, sources, and streams.
- `pactl info` shows the PulseAudio-compatible server information, if the
  `pactl` tool is installed. `wpctl` is the primary PipeWire tool.

Open **Settings → Sound** and confirm the correct output device is selected. If
the device is missing, check `lspci -k` for the audio device and its driver.

For a terminal mixer, run:

```bash
alsamixer
```

Use the arrow keys to select a channel and `M` to unmute it. Press `Esc` to quit.

### DNF is locked or a transaction is already running

If DNF reports a lock or a running transaction, wait for it to finish or restart
the computer. Do not delete DNF lock files.

### DNF reports a repository error

- Read the repository name in the error.
- Check the internet connection.
- Confirm that the repository supports Fedora 45.
- Disable an unavailable or outdated third-party repository.
- Do not mix packages from different Fedora releases.

Disable a repository with DNF5:

```bash
sudo dnf config-manager setopt REPOSITORY-ID.enabled=0
```

Replace `REPOSITORY-ID` with the ID from `dnf repolist`. To re-enable it, use
`.enabled=1`.

If `config-manager` is missing, install the DNF5 plugins:

```bash
sudo dnf install dnf5-plugins
```

### DNF reports `NOKEY` or a signature error

Fedora 45 enforces RPM signature checking by default.

- Confirm that the repository's GPG key is installed correctly.
- Reinstall the repository release package from its official source.
- Do not use `--nogpgcheck` or `--nosignature` to bypass the error.
- If the error comes from a third-party repository, check that repository's
  current instructions.

### Fedora's repository files are missing from `/etc/yum.repos.d/`

Fedora 45 moved its own repository configuration to
`/usr/share/dnf5/repos.d/`. Third-party repositories usually remain in
`/etc/yum.repos.d/`. Use:

```bash
dnf repolist
```

This shows what DNF is actually using.

### A DNF transaction removed or changed a package I need

Review the DNF history:

```bash
sudo dnf history list
```

To undo a transaction, use its ID:

```bash
sudo dnf history undo ID
```

> **Warning:** Undoing a transaction can remove or downgrade packages that were
> installed later. Read the transaction list before confirming. If you are
> unsure, reinstall the specific package instead.

### Flatpak cannot find an application

- Check the remotes:
  ```bash
  flatpak remotes
  ```
- Update metadata:
  ```bash
  flatpak update --appstream
  ```
- Search with the application ID or name:
  ```bash
  flatpak search gimp
  ```

If Flathub is missing, add it:

```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

### SELinux denies an application

Do not set SELinux to permissive as a first step. Check the denial:

```bash
sudo ausearch -m AVC -ts recent
```

If `setroubleshoot-server` is installed, you can also use:

```bash
sudo sealert -a /var/log/audit/audit.log
```

Follow Fedora's SELinux troubleshooting documentation for the specific denial.
Fix the file label, boolean, or policy instead of disabling SELinux.

### The firewall blocks a service

Check whether the service is listening:

```bash
sudo ss -tlnp
```

Check the active firewall zone:

```bash
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --list-all
```

Open only the port or service you need. For example:

```bash
sudo firewall-cmd --add-port=8080/tcp
```

This is a runtime-only rule. Add `--permanent` and run `sudo firewall-cmd
--reload` if you want it to survive a reboot. Remove rules you no longer need.

### The computer does not boot after installation

- Check the boot order in the firmware settings.
- For a dual-boot system, check whether Windows Fast Startup or BitLocker is
  involved before changing boot settings.
- Boot a live USB and back up your files before attempting a repair.
- Use Fedora's official recovery or chroot instructions. Do not run random
  boot-repair scripts from the internet.

### Windows is missing from the boot menu

```bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

This rebuilds the GRUB configuration. On UEFI systems, the output path may be
`/boot/efi/EFI/fedora/grub.cfg`. Read the output for a Windows entry.

If Windows still does not appear, do not run random repair scripts. Read
[Dual-booting Linux and Windows](../guides/dual-boot.md) and back up your files
first.

> **BitLocker warning:** Changing boot order or firmware settings can trigger a
> BitLocker recovery prompt. Have the recovery key before making changes.

### A Fedora release upgrade fails

Review the upgrade log:

```bash
sudo dnf system-upgrade log
```

If the downloaded packages are causing problems, cancel the queued upgrade:

```bash
sudo dnf offline clean
```

Then read the Fedora release notes and try again. Do not skip several Fedora
releases at once. Official upgrades are tested across at most two releases.

### The text console has the wrong keyboard layout

Fedora 45 uses `kmscon` for the text console. `setfont` and `loadkeys` do not
work the same way as on older consoles. Set the console keymap with:

```bash
sudo localectl set-keymap us
```

Replace `us` with your keymap. This changes the console layout, not the GNOME
desktop layout.

### An application does not start

1. Open a terminal and run the application from there to see the error message.
2. Check free disk space.
3. Update the application and the system.
4. Remove recently installed extensions or themes if the problem started after a
   desktop change.
5. Reinstall only the affected package:
   ```bash
   sudo dnf reinstall package-name
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
- Ask for help with the exact error, Fedora version, kernel version, and the
  steps you already tried.

## Uninstall or Restore

### Undo a package change

Remove a package you added:

```bash
sudo dnf remove package-name
```

Read the transaction list. Removing a package can also remove applications that
depend on it.

### Undo a driver change

Open the RPM Fusion or Fedora documentation for your driver. For NVIDIA, remove
the RPM Fusion NVIDIA packages and reboot. If the desktop does not start, use
recovery mode or a text console.

### Restore the system

Fedora Workstation does not create automatic Btrfs snapshots by default. If you
created a backup or snapshot before the change, restore it with the same tool.

If Fedora cannot boot, use a live USB and follow
[Backup and restore](../maintenance/backup-and-restore.md). The Fedora installer
also offers a **Reinstall Fedora** option that can keep `/home`, but it is not a
backup and can still fail.

### Reinstall Fedora

Reinstalling is a last resort. Back up your files first. The Fedora installer can
replace the system, but it can also erase the disk if you choose the wrong
option. Read [Installation](./03-installation.md) again before starting.

## References

- [Fedora 45 release notes](https://docs.fedoraproject.org/en-US/release-notes/f45/)
- [Fedora 45 common issues](https://discussion.fedoraproject.org/tags/c/ask/common-issues/82/all/f45)
- [Fedora Workstation documentation](https://docs.fedoraproject.org/en-US/workstation-docs/)
- [Fedora Quick Docs: DNF](https://docs.fedoraproject.org/en-US/quick-docs/dnf/)
- [Fedora Quick Docs: SELinux](https://docs.fedoraproject.org/en-US/quick-docs/selinux-getting-started/)
- [Fedora Quick Docs: DNF system upgrade](https://docs.fedoraproject.org/en-US/quick-docs/dnf-system-upgrade/)
- [firewalld documentation](https://firewalld.org/documentation/)
- [Ask Fedora](https://ask.fedoraproject.org/)
- [Fedora Bugzilla guidance](https://fedoraproject.org/wiki/Bugzilla)
- [General troubleshooting hub](../troubleshooting/README.md)
