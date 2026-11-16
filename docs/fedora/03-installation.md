# Installation — Fedora

**Supported version:** Fedora Workstation 45 (x86_64 and aarch64)

**Difficulty:** Beginner

**Terminal required:** No

**Last reviewed:** Not yet reviewed on live systems.

**Learning path:** ← [Download](./02-download.md) · [Fedora overview](./README.md) · [Initial Setup](./04-initial-setup.md) →

## Overview

This tutorial installs Fedora Workstation 45 as the only operating system on a
computer, or replaces an existing system. It uses the Anaconda Web UI installer
included on the Fedora Workstation Live image.

If you want to keep Windows, read
[Dual-booting Linux and Windows](../guides/dual-boot.md) first and choose the
option to share the disk when the installer offers it.

> **Warning:** Installing Fedora can erase every file on the selected disk. Back
> up important files to a separate drive before you start. Check twice that you
> selected the correct disk.

This tutorial is for **Fedora Workstation**, not Fedora Atomic Desktops such as
Silverblue or Kinoite. Atomic desktops use a different installation and update
model.

## Requirements

- A computer that meets the Fedora Workstation requirements. For a comfortable
  desktop, use at least 8 GB RAM and 40 GB of free disk space.
- A verified Fedora Workstation USB drive. See [Download Fedora](./02-download.md)
  and [Create a bootable USB drive](../guides/bootable-usb.md).
- A backup of important files on a separate drive.
- The computer connected to power. Do not install on battery power if you can
  avoid it.
- An internet connection if you want updates and third-party repositories during
  or after installation.
- Your user name, computer name, and password ready.

> **BitLocker and Windows:** If Windows uses BitLocker, save the BitLocker
> recovery key before changing firmware or boot settings. A recovery prompt can
> appear after the boot order changes.

## Installation

### Step 1: Boot from the USB drive

1. Shut down the computer completely.
2. Insert the Fedora USB drive.
3. Turn the computer on and open the **one-time boot menu**. Common keys are
   `F12`, `F11`, `Esc`, or `F9`. The correct key depends on the manufacturer.
4. Select the USB drive. It may appear as `UEFI: <drive name>` or
   `USB HDD: <drive name>`.
5. On the Fedora boot menu, choose **Start Fedora Workstation Live**.

The boot menu may also offer a media check. You can run it if you want to test
the USB before installation, but it adds time.

If the USB does not appear, see the
[bootable USB troubleshooting section](../guides/bootable-usb.md#troubleshooting).

### Step 2: Start the installer from the live desktop

Fedora Workstation boots into a live GNOME desktop. Nothing is written to your
disk yet.

1. A welcome dialog usually offers **Install Fedora Linux** and **Not Now**.
2. Choose **Install Fedora Linux**.
3. If you closed the dialog, open **Activities**, search for **Install to Hard
   Drive**, and open it.

You can use the live desktop to test Wi-Fi, sound, display, keyboard, and
touchpad before installing.

### Step 3: Choose language and keyboard

1. Select your language on the welcome screen.
2. Open the keyboard layout option and choose the layout that matches your
   physical keyboard.
3. Test the layout by typing in the preview if one is shown.
4. Click **Next**.

Fedora 45 uses a four-step installer: **Welcome**, **Installation method**,
**Storage configuration**, and **Review and install**. The labels can change
slightly between releases, so read each screen.

### Step 4: Choose the installation method

The installer lists the options that fit the selected disk.

| Option | What it does | Use it when |
|---|---|---|
| **Use entire disk** | Erases the selected disk and installs Fedora on it. | The disk contains no data you need, or you have a full backup and want only Fedora. |
| **Share disk with other operating systems** | Keeps an existing system and installs Fedora in free space. | You want to keep Windows or another Linux system. Back up first. |
| **Reinstall Fedora** | Replaces an existing Fedora system and can keep files in `/home`. | Fedora is already installed and you want a clean system while keeping home files. |
| **Use free space** | Installs only into unallocated space. | The disk already has free space and you do not want to shrink a partition. |

> **Warning:** **Use entire disk** destroys the selected disk's contents. If the
> computer has more than one disk, confirm that the installer selected the disk
> you intend to erase.

> **Manual storage warning:** The storage editor can change partitions
> immediately and has no undo. Use it only if you understand the layout you need.
> Do not delete a partition unless you know what it contains.

If **Share disk with other operating systems** is missing, the disk may not have
enough free space, may use an unsupported layout, or may be encrypted. Do not
force the installer. Read the [dual-boot guide](../guides/dual-boot.md) or ask
for help.

### Step 5: Configure storage and encryption

Fedora uses **Btrfs** by default. A typical automatic layout creates:

| Partition or subvolume | Purpose |
|---|---|
| EFI system partition | Boot files for UEFI systems (`/boot/efi`) |
| `/boot` | Kernel and initramfs files |
| Btrfs `/` subvolume | The Fedora system |
| Btrfs `/home` subvolume | User files, sharing free space with `/` |

The exact sizes depend on the disk.

You can enable **Encrypt my data** and set a passphrase. Encryption protects your
files if the computer is lost or stolen. You must enter the passphrase every time
the computer starts.

> **Encryption warning:** There is no password recovery for a full-disk
> encryption passphrase. Store a copy somewhere safe. Also check the boot
> keyboard layout before setting the passphrase; the prompt uses that layout.

### Step 6: Review the installation

1. Check the selected disk and the summary of changes.
2. Set a hostname if you want a specific computer name on the network. Otherwise,
   the network can assign one.
3. Confirm that you have a backup of anything important on the target disk.
4. If anything looks wrong, go back and change it.
5. Click **Erase data and install** or **Install**, depending on the chosen
   method. Read the final warning before confirming.

### Step 7: Wait for the installation

1. Wait while Fedora writes the system. Do not close the lid, unplug power, or
   remove the USB drive.
2. When the installer reports success, choose **Reboot**.
3. Remove the USB drive when the screen tells you to.
4. Press `Enter` if the system asks you to remove the media and continue.

If the computer boots back into the live session, remove the USB and check the
boot order in the firmware settings.

### Step 8: Complete GNOME Initial Setup

Fedora Workstation creates the user account after the first reboot, not inside
the installer.

1. Choose your language and keyboard layout.
2. Connect to a network.
3. Set your time zone.
4. Create your user account and password.
5. Decide whether to enable third-party repositories. This is optional and
   includes sources such as Flathub and vendor repositories.
6. Review the privacy settings and finish setup.

Your user account is added to the `wheel` group, which allows `sudo` for
administrative commands.

## Configuration

The installer sets the basics. Complete these steps on the first boot:

1. Sign in with the account you created.
2. Connect to the network if you did not do so during initial setup.
3. Check **Settings → Displays** and set the correct resolution and scale.
4. Check **Settings → Sound** and test the speakers or headphones.
5. Check **Settings → Power** and set a screen blank timeout that suits you.
6. Open a terminal and run the first updates from
   [Initial Setup](./04-initial-setup.md).

### Disk encryption

If you enabled encryption, Fedora asks for the passphrase before the desktop
starts. Keep a recovery copy of the passphrase. Losing it means losing access to
the data.

### Dual boot

If you installed alongside Windows:

- Check that both systems appear in the firmware boot menu.
- Keep backups of both systems.
- Do not change BitLocker or Secure Boot settings without reading the
  [dual-boot guide](../guides/dual-boot.md).

### Snapshots

Fedora Workstation does not set up automatic Btrfs snapshots by default. If you
want snapshots, configure a supported snapshot tool after installation and test
it. A snapshot is not a substitute for a backup on a separate drive.

## Verification

After the first boot, open a terminal and run:

```bash
cat /etc/os-release
```

You should see `Fedora Linux` and a `VERSION_ID` that matches Fedora 45.

Check the disk layout:

```bash
lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINTS
findmnt -no FSTYPE /
```

- `lsblk` lists disks, partitions, and mount points.
- `findmnt -no FSTYPE /` shows the filesystem for the root directory. On a
  default Fedora Workstation install, it usually prints `btrfs`.

Check that the system boots without the USB:

1. Shut down.
2. Remove the USB drive.
3. Start the computer and confirm that Fedora starts normally.

Check the basics:

- [ ] Wi-Fi or Ethernet works.
- [ ] Sound works.
- [ ] The display resolution is correct.
- [ ] The keyboard and touchpad work.
- [ ] You can sign out and sign back in.
- [ ] Updates can be downloaded.

## Troubleshooting

### The computer does not boot from the USB

- Open the boot menu, not only the firmware settings.
- Try another USB port, preferably a rear port on a desktop.
- Recreate the USB with Fedora Media Writer.
- Check whether the USB was written in UEFI or legacy mode and match the
  computer's firmware mode.
- Check the ISO checksum again.

### The installer cannot find a disk

- Some laptops use Intel RST/RAID mode. Changing it to AHCI may be required, but
  it can break an existing Windows installation. Back up and follow the laptop
  manufacturer's instructions first.
- Check that the disk is visible in the firmware settings.
- If the disk is encrypted with BitLocker, do not delete it from the installer.
  Boot Windows and shrink or decrypt the volume properly first.

### The screen is black or the installer freezes

- Wait a few minutes; some graphics cards take time to initialize.
- Try the basic graphics mode if the boot menu offers it.
- Disconnect extra monitors and try the built-in display.
- On NVIDIA hardware, use the open-source driver first. See
  [Graphics drivers](../guides/graphics-drivers.md).

### Secure Boot blocks the USB

- Fedora supports Secure Boot on compatible hardware.
- Check the official Fedora installation and release notes for your release.
- Do not disable Secure Boot as a first step. If you must change it, note the
  current setting and any BitLocker recovery requirements first.

### The installation stops with a storage error

- Do not force a shutdown unless the installer is completely frozen.
- Note the exact error message.
- Restart the live USB and check the disk health.
- Verify the ISO checksum and recreate the USB.
- Check the Fedora 45 release notes and common issues page.

### Windows no longer appears in the boot menu

- Check the firmware boot menu before changing Linux boot files.
- Boot Fedora and run:
  ```bash
  sudo grub2-mkconfig -o /boot/grub2/grub.cfg
  ```
  This rebuilds the GRUB configuration. On UEFI systems, the file path may be
  `/boot/efi/EFI/fedora/grub.cfg`. Do not run random repair scripts.
- Back up your files before attempting a bootloader repair.

### I forgot the encryption passphrase

There is no supported way to recover the data without the passphrase. A live USB
can erase and reinstall the system, but the encrypted data remains inaccessible.
Contact professional data recovery only if the data is critical, and understand
that success is not guaranteed.

## Uninstall or Restore

Fedora does not have an uninstall button. Removing it depends on how it was
installed.

### Fedora is the only system

1. Back up your personal files to a separate drive.
2. Create installation media for the operating system you want to use instead.
3. Boot that installer and follow its instructions. It will replace Fedora.

### Fedora is installed alongside Windows

1. Back up files from both systems.
2. Boot Windows and use **Disk Management** to identify the Fedora partitions.
3. Do not delete a partition unless you are sure it belongs to Fedora. Windows
   partitions usually use NTFS; Fedora often uses Btrfs, ext4, and a shared EFI
   partition.
4. Delete only the Fedora data partitions. Do not delete the shared EFI system
   partition.
5. Remove the Fedora boot entry from the firmware boot menu if it remains.
6. Repair the Windows bootloader only if Windows does not start. Follow official
   Microsoft or computer-manufacturer instructions.

> **Warning:** Deleting the wrong partition can destroy Windows or your personal
> files. If you are unsure, stop and ask for help before deleting anything.

### Restore a previous system

If you created a backup before installing, restore it with the same tool. See
[Backup and restore](../maintenance/backup-and-restore.md). Fedora does not create
a system snapshot automatically during installation.

## References

- [Fedora 45 release notes](https://docs.fedoraproject.org/en-US/release-notes/f45/)
- [Fedora Workstation documentation](https://docs.fedoraproject.org/en-US/workstation-docs/)
- [Anaconda Web UI installation flow](https://anaconda-installer.readthedocs.io/en/latest/anaconda-webui/docs/installation-steps.html)
- [Preparing boot media](https://docs.fedoraproject.org/en-US/quick-docs/creating-and-using-a-live-installation-image/)
- [Getting started with Fedora](https://docs.fedoraproject.org/en-US/quick-docs/getting-started-guide/)
- [Create a bootable USB drive](../guides/bootable-usb.md)
- [Dual-booting Linux and Windows](../guides/dual-boot.md)

**Next:** [Complete the initial setup](./04-initial-setup.md)
