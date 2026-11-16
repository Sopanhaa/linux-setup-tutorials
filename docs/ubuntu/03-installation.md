# Installation — Ubuntu

**Supported version:** Ubuntu Desktop 26.04 LTS (amd64)

**Difficulty:** Beginner

**Terminal required:** No

**Last reviewed:** Not yet reviewed on live systems.

**Learning path:** ← [Download](./02-download.md) · [Ubuntu overview](./README.md) · [Initial Setup](./04-initial-setup.md) →

## Overview

This tutorial installs Ubuntu Desktop as the only operating system on a
computer, or replaces an existing system. It uses the graphical Ubuntu installer.

If you want to keep Windows, read
[Dual-booting Linux and Windows](../guides/dual-boot.md) first and choose the
**Install alongside** option when the installer offers it.

> **Warning:** Installing Ubuntu can erase every file on the selected disk. Back
> up your important files to a separate drive before you start. Check twice that
> you selected the correct disk.

## Requirements

- A computer that meets the Ubuntu Desktop requirements:
  - 2 GHz dual-core processor or better
  - 6 GB RAM or more
  - 25 GB of free disk space or more
  - a USB port
- A verified Ubuntu USB drive. See [Download Ubuntu](./02-download.md) and
  [Create a bootable USB drive](../guides/bootable-usb.md).
- A backup of important files on a separate drive.
- The computer connected to power. Do not install on battery power if you can
  avoid it.
- An internet connection if you want updates and third-party drivers during
  installation. The installer can work offline, but you will need to update
  afterwards.
- Your user name, computer name, and password ready.

> **BitLocker and Windows:** If the computer has Windows with BitLocker, save
> the BitLocker recovery key before changing anything. Firmware or boot changes
> can trigger a recovery prompt.

## Installation

### Step 1: Boot from the USB drive

1. Shut down the computer completely.
2. Insert the Ubuntu USB drive.
3. Turn the computer on and open the **one-time boot menu**. Common keys are
   `F12`, `F11`, `Esc`, or `F9`. The correct key depends on the manufacturer.
4. Select the USB drive. It may appear as `UEFI: <drive name>` or
   `USB HDD: <drive name>`.
5. If the computer boots straight into the installed system, restart and try the
   boot-menu key again, or change the boot order in the firmware settings.

If the USB does not appear, see the
[bootable USB troubleshooting section](../guides/bootable-usb.md#troubleshooting).

### Step 2: Choose Try Ubuntu or Install Ubuntu

The live environment opens a desktop with two main choices:

- **Try Ubuntu** tests Ubuntu without installing. Use it to check Wi-Fi, sound,
  display, keyboard, and touchpad.
- **Install Ubuntu** starts the installer.

Choose **Install Ubuntu** when you are ready.

### Step 3: Choose language, keyboard, and network

1. Select your language and click **Next**.
2. Choose your keyboard layout. Test it by typing in the preview box.
3. Connect to Wi-Fi or plug in an Ethernet cable. A network connection lets the
   installer download updates and drivers.
4. Choose whether to install third-party software for graphics and Wi-Fi
   hardware, and for additional media formats. This is usually recommended, but
   some hardware works better with the open-source driver. You can change this
   later.

### Step 4: Choose the installation type

The exact labels can change between Ubuntu releases. Read each option carefully.

| Option | What it does | Use it when |
|---|---|---|
| **Erase disk and install Ubuntu** | Deletes the selected disk and installs Ubuntu on it. | The disk contains no data you need, or you have a full backup and want only Ubuntu. |
| **Install alongside** | Shrinks an existing system and installs Ubuntu next to it. | You want to keep Windows or another system. Back up first. |
| **Manual installation** | Lets you create and select partitions yourself. | You know how partitioning works or need a custom layout. |
| **Encrypt the new Ubuntu installation** | Encrypts the Ubuntu disk with LUKS. | You want protection if the device is lost or stolen. You must remember the passphrase. |

> **Warning:** **Erase disk** does not ask you to confirm every file. It destroys
> the selected disk's contents. If the computer has more than one disk, make sure
> the installer selected the disk you intend to erase.

> **Manual partitioning warning:** Do not change or delete a partition unless you
> know what it contains. Changing the wrong partition can destroy your Windows
> installation or personal files.

If **Install alongside** is missing, the disk may not have enough free space, may
use an unsupported partition layout, or may be encrypted. Do not force the
installer. Read the [dual-boot guide](../guides/dual-boot.md) or ask for help.

### Step 5: Review the disk changes

The installer shows a summary of the changes it will make.

- Read the list of partitions and disks.
- Confirm that the target disk is correct.
- Confirm that you have a backup of anything important on that disk.
- If anything looks wrong, click **Back** and change it.

### Step 6: Choose time zone and create your account

1. Select your time zone. The installer usually detects it from the network.
2. Enter your name, computer name, user name, and password.
3. Choose a strong password. You will use it for `sudo` and for unlocking the
   computer.
4. Choose whether to require a password at login. For a shared or portable
   computer, keep automatic login off.

### Step 7: Start the installation

1. Click **Install**.
2. Wait while files are copied. Do not close the lid, unplug the power, or remove
   the USB drive.
3. If the installer asks about a disk that is in use, read the message before
   continuing.
4. When installation finishes, choose **Restart Now**.
5. Remove the USB drive when the screen tells you to, then press `Enter`.

If the computer boots back into the installer, remove the USB and check the boot
order in the firmware settings.

## Configuration

The installer sets the basics. Complete these steps on the first boot:

1. Sign in with the account you created.
2. Connect to the network if you did not do so during installation.
3. Open **Software & Updates** and choose a nearby download server if downloads
   are slow.
4. Install updates. The next page, [Initial Setup](./04-initial-setup.md), walks
   through this.
5. Check **Settings → Displays** and set the correct resolution and scale.
6. Check **Settings → Sound** and test the speakers or headphones.
7. Open **Settings → Power** and set a screen blank timeout that suits you.

### Disk encryption

If you enabled encryption, you will enter the encryption passphrase before
Ubuntu starts. Keep a recovery copy of the passphrase somewhere safe. Losing it
means losing access to the data.

### Dual boot

If you installed alongside Windows:

- Check that both systems appear in the boot menu.
- Do not disable Windows Fast Startup or BitLocker without reading the
  [dual-boot guide](../guides/dual-boot.md).
- Keep backups of both systems.

## Verification

After the first boot, open a terminal with `Ctrl` + `Alt` + `T` and run:

```bash
cat /etc/os-release
```

You should see `NAME="Ubuntu"` and a `VERSION_ID` that matches the release you
installed.

Check the disk layout:

```bash
lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINT
```

- `lsblk` lists disks and partitions.
- `FSTYPE` shows filesystems such as `ext4`, `vfat`, or `crypto_LUKS`.
- `MOUNTPOINT` shows which partition is mounted at `/`.

Check that the system boots without the USB:

1. Shut down.
2. Remove the USB drive.
3. Start the computer and confirm that Ubuntu starts normally.

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
- Recreate the USB with Rufus or Ventoy.
- Check whether the USB was written in UEFI or legacy mode and match the
  computer's firmware mode.
- Check the ISO checksum again.

### The installer cannot find a disk

- Some laptops use Intel RST/RAID mode. Changing it to AHCI may be required, but
  it can break an existing Windows installation. Back up and follow the laptop
  manufacturer's instructions first.
- Check that the disk is visible in the firmware settings.
- Try the latest Ubuntu release or a newer kernel.

### The screen is black or the installer freezes

- Wait a few minutes; some graphics cards take time to initialize.
- Try booting the USB in safe graphics mode if the boot menu offers it.
- Disconnect extra monitors and try the built-in display.
- On NVIDIA hardware, try the open-source driver first. See
  [Graphics drivers](../guides/graphics-drivers.md).

### Secure Boot blocks the USB

- Check whether Ubuntu's official documentation supports your computer and
  release with Secure Boot enabled.
- Do not disable Secure Boot as a first step. If you must change it, note the
  current setting and any BitLocker recovery requirements first.

### The installation stops with a disk or package error

- Do not force a shutdown unless the installer is completely frozen.
- Note the exact error message.
- Restart the live USB and check the disk health.
- Verify the ISO checksum and recreate the USB.
- Check the official Ubuntu release notes for known installer issues.

### Windows no longer appears in the boot menu

- Boot Ubuntu and run:
  ```bash
  sudo os-prober
  sudo update-grub
  ```
  `os-prober` looks for other operating systems. `update-grub` rebuilds the boot
  menu. Read the output before restarting.
- If Windows still does not appear, do not run random repair scripts. Read the
  [dual-boot guide](../guides/dual-boot.md) and back up your files first.

## Uninstall or Restore

Ubuntu does not have an uninstall button. Removing it depends on how it was
installed.

### Ubuntu is the only system

1. Back up your personal files to a separate drive.
2. Create installation media for the operating system you want to use instead.
3. Boot that installer and follow its instructions. It will replace Ubuntu.

### Ubuntu is installed alongside Windows

1. Back up files from both systems.
2. Boot Windows and use **Disk Management** to identify the Ubuntu partitions.
3. Do not delete a partition unless you are sure it belongs to Ubuntu. Windows
   partitions usually use NTFS; Ubuntu often uses ext4 and a small EFI partition
   shared with Windows.
4. Delete only the Ubuntu data partition and, if you are removing Ubuntu
   completely, remove its boot entry with your firmware's boot menu or a Windows
   tool.
5. Repair the Windows bootloader only if Windows does not start. Follow official
   Microsoft or computer-manufacturer instructions.

> **Warning:** Deleting the wrong partition can destroy Windows or your personal
> files. If you are unsure, stop and ask for help before deleting anything.

### Restore a previous system

If you created a Timeshift snapshot or a full backup before installing, restore
it with the same tool. See
[Backup and restore](../maintenance/backup-and-restore.md).

## References

- [Install Ubuntu Desktop](https://ubuntu.com/tutorials/install-ubuntu-desktop)
- [Ubuntu Desktop documentation](https://ubuntu.com/desktop/docs/)
- [Ubuntu release notes](https://documentation.ubuntu.com/release-notes/)
- [Ubuntu installation requirements](https://ubuntu.com/desktop/docs/)
- [Create a bootable USB drive](../guides/bootable-usb.md)
- [Dual-booting Linux and Windows](../guides/dual-boot.md)

**Next:** [Complete the initial setup](./04-initial-setup.md)
