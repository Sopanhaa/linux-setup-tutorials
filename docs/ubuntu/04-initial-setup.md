# Initial Setup — Ubuntu

**Supported version:** Ubuntu Desktop 26.04 LTS

**Difficulty:** Beginner

**Terminal required:** Yes for the update and driver commands.

**Last reviewed:** Not yet reviewed on live systems.

**Learning path:** ← [Installation](./03-installation.md) · [Ubuntu overview](./README.md) · [Desktop Setup](./05-desktop-setup.md) →

## Overview

This tutorial turns a fresh Ubuntu installation into a usable daily system. You
will install updates, check drivers, add media support, enable a firewall, and
set up backups.

The commands use Ubuntu's official package manager, `apt`. Run them in a terminal
opened with `Ctrl` + `Alt` + `T`.

## Requirements

- Ubuntu Desktop installed and able to boot.
- An internet connection.
- Your user account must be able to run `sudo`. The first user account created
  during installation normally can.
- At least 5 GB of free disk space for updates and applications.
- A backup plan for your personal files.

> **Before you change drivers or firmware:** connect the computer to power. A
> power loss during a firmware update can make the device unusable.

## Installation

### Step 1: Update the package list and installed packages

```bash
sudo apt update
sudo apt upgrade
```

- `sudo` runs the command as an administrator.
- `apt update` downloads the current list of available packages. It does not
  install anything.
- `apt upgrade` installs newer versions of the packages already on the system.

When `apt upgrade` asks for confirmation, read the list and type `Y` to continue.
If a kernel or driver update is included, restart after the upgrade finishes.

To restart:

```bash
sudo reboot
```

### Step 2: Remove obsolete packages

After updates, some old packages may no longer be needed:

```bash
sudo apt autoremove
```

`apt autoremove` removes packages that were installed automatically and are no
longer required. Read the list before confirming. It can remove old kernels, but
it will not remove your personal files.

### Step 3: Check hardware drivers

Ubuntu can detect additional drivers for graphics and Wi-Fi hardware.

```bash
ubuntu-drivers devices
```

This lists devices and recommends drivers. It only reads information.

To install the recommended drivers:

```bash
sudo ubuntu-drivers install
```

If you use NVIDIA graphics, read [Graphics drivers](../guides/graphics-drivers.md)
before installing. Secure Boot may require enrolling a key, and the wrong driver
can prevent the desktop from starting.

You can also open **Software & Updates → Additional Drivers** and choose a driver
there.

### Step 4: Install media codecs (optional)

Some video and audio formats are not installed by default because of licensing.

```bash
sudo apt install ubuntu-restricted-extras
```

This package includes common codecs and fonts. The installer may show a license
agreement. Read it before accepting. If you do not need extra media support, skip
this step.

### Step 5: Install Flatpak and Flathub (optional)

Ubuntu includes Snap by default. Flatpak is another way to install desktop
applications.

```bash
sudo apt install flatpak gnome-software-plugin-flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

- `flatpak` is the Flatpak runtime.
- `gnome-software-plugin-flatpak` lets GNOME Software show Flatpak apps.
- `flatpak remote-add` adds Flathub as a software source. `--if-not-exists`
  prevents an error if it is already added.

Restart or sign out and back in so the software center picks up the new source.
See [Package managers](../guides/package-managers.md#flatpak-and-snap) for more
details.

### Step 6: Enable the firewall

Ubuntu includes `ufw`, the Uncomplicated Firewall. It is usually inactive after
installation.

Check its status:

```bash
sudo ufw status
```

If it says `Status: inactive`, you can enable it:

```bash
sudo ufw enable
```

> **SSH warning:** If you use SSH to connect to this computer, allow SSH before
> enabling the firewall:
> ```bash
> sudo ufw allow ssh
> ```
> Otherwise, enabling the firewall can disconnect your remote session.

Check the result:

```bash
sudo ufw status verbose
```

### Step 7: Set up backups

Install a backup tool and create the first backup:

```bash
sudo apt install deja-dup timeshift
```

- Déjà Dup backs up personal files.
- Timeshift creates system snapshots.

See [Backup and restore](../maintenance/backup-and-restore.md) for setup,
verification, and restore steps. Do not skip the restore test.

### Step 8: Check firmware updates (optional)

Many Ubuntu systems can update firmware through `fwupd`:

```bash
sudo fwupdmgr refresh
sudo fwupdmgr get-updates
```

- `refresh` downloads the latest firmware metadata.
- `get-updates` lists available updates without installing them.

If updates are listed, read the details and connect the computer to power before
running:

```bash
sudo fwupdmgr update
```

Do not unplug power or force a shutdown during a firmware update. Some firmware
updates restart the computer automatically.

## Configuration

### Choose automatic updates

Open **Software & Updates → Updates** and review:

- **Important security updates** should stay enabled.
- **Recommended updates** are usually safe to enable.
- **Proposed updates** are for testing and are not recommended for daily use.

Ubuntu installs security updates automatically by default. You can review the
setting in **Settings → Ubuntu Software** or in the software center, depending on
your release.

### Configure the time zone

Check the time zone:

```bash
timedatectl
```

If it is wrong, set it in **Settings → Date & Time**, or list available zones:

```bash
timedatectl list-timezones
```

Then set your zone. For example:

```bash
sudo timedatectl set-timezone Asia/Phnom_Penh
```

Replace `Asia/Phnom_Penh` with your own zone from the list.

### Check Snap

Ubuntu ships with Snap:

```bash
snap list
```

This lists installed Snap packages. Snap applications update automatically. You
can manage them in **Ubuntu Software** or with the `snap` command.

### Configure backups

- In Déjà Dup, choose the folders to back up and a separate drive.
- In Timeshift, choose a snapshot location and schedule.
- Keep at least one backup copy on a drive that is not always connected.

## Verification

Run these checks after the setup:

```bash
sudo apt update
sudo ufw status
flatpak --version
snap list
```

Expected results:

- `apt update` finishes without errors.
- `ufw status` shows `Status: active`.
- `flatpak --version` prints a version number if you installed Flatpak.
- `snap list` shows the installed Snap packages.

Check the driver in use:

```bash
lspci -k | grep -A 3 -E 'VGA|3D'
```

This shows your graphics device and the kernel driver. See
[Graphics drivers](../guides/graphics-drivers.md#verification) for how to read the
output.

Restart the computer and confirm that:

- [ ] The desktop starts normally.
- [ ] Networking works.
- [ ] Sound works.
- [ ] The firewall is still active.
- [ ] Your backup tool can see the backup drive.

## Troubleshooting

### `Could not get lock /var/lib/dpkg/lock-frontend`

Another package operation is running. Wait for it to finish, or restart the
computer. Do not delete the lock file.

### `apt update` reports a repository error

- Check the exact URL in the error.
- Confirm that you have an internet connection.
- Disable a third-party repository you added if it is no longer available.
- Do not mix repositories from different Ubuntu releases.

### A package installation was interrupted

```bash
sudo dpkg --configure -a
sudo apt --fix-broken install
```

- `dpkg --configure -a` finishes package setups that were interrupted.
- `apt --fix-broken install` tries to repair broken dependencies.

Read the output before continuing. If the messages mention deleting packages you
need, stop and ask for help.

### The NVIDIA driver causes a black screen

- Boot an older kernel from the GRUB menu or use recovery mode.
- Remove the NVIDIA driver and return to the open-source driver.
- Read [Graphics drivers](../guides/graphics-drivers.md#troubleshooting).

### The firewall blocks an application

Check the rules:

```bash
sudo ufw status numbered
```

Do not disable the firewall permanently. Add a rule for the specific port or
application you need, or ask the application's documentation which ports it uses.

### Flatpak applications do not appear in GNOME Software

- Confirm that the Flathub remote exists:
  ```bash
  flatpak remotes
  ```
- Sign out and back in.
- Update the software center:
  ```bash
  sudo apt update
  sudo apt upgrade
  ```

## Uninstall or Restore

### Remove optional packages

Remove only the packages you added and no longer need. For example:

```bash
sudo apt remove flatpak gnome-software-plugin-flatpak
sudo apt remove deja-dup timeshift
```

`apt remove` removes the program but may leave configuration files. Use
`sudo apt purge <package>` only when you also want to remove system-wide
configuration.

### Disable the firewall

```bash
sudo ufw disable
```

Only do this if you understand the risk. A firewall protects against unwanted
incoming connections.

### Undo a driver change

Open **Software & Updates → Additional Drivers** and select the previous driver,
or use the recovery steps in [Graphics drivers](../guides/graphics-drivers.md).

### Restore personal files

Use Déjà Dup or `rsync` to restore from your backup. See
[Backup and restore](../maintenance/backup-and-restore.md#uninstall-or-restore).

## References

- [Ubuntu Desktop documentation](https://ubuntu.com/desktop/docs/)
- [Ubuntu Server documentation](https://ubuntu.com/server/docs)
- [Ubuntu package management](https://ubuntu.com/server/docs/package-management)
- [Ubuntu community help](https://help.ubuntu.com/community/)
- [Ubuntu drivers](https://ubuntu.com/server/docs/nvidia-drivers-installation)
- [Flatpak documentation](https://docs.flatpak.org/)
- [Snap documentation](https://snapcraft.io/docs)
- [UFW documentation](https://help.ubuntu.com/community/UFW)
- [fwupd documentation](https://fwupd.org/)

**Next:** [Set up your desktop](./05-desktop-setup.md)
