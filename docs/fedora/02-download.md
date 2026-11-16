# Download — Fedora

**Supported version:** Fedora Workstation 45 (x86_64 and aarch64)

**Difficulty:** Beginner

**Terminal required:** No, but checksum verification uses a terminal or PowerShell.

**Last reviewed:** Not yet reviewed on live systems.

**Learning path:** ← [Introduction](./01-introduction.md) · [Fedora overview](./README.md) · [Installation](./03-installation.md) →

## Overview

This tutorial helps you download the correct Fedora Workstation image, verify it,
and write it to a USB drive.

By the end, you will have installation media that can boot your computer. You
will not install Fedora yet. That is the next page:
[Installation](./03-installation.md).

Fedora recommends **Fedora Media Writer** for writing the image. This tutorial
uses Fedora Media Writer as the primary method and links to the project's
[Create a bootable USB drive](../guides/bootable-usb.md) guide for Rufus and
Ventoy.

## Requirements

- A computer with internet access.
- A USB drive with at least 4 GB of free space. **Use 8 GB or larger for a
  comfortable margin. Writing the image erases the USB drive.**
- Enough free disk space for the ISO file (about 3 GB).
- A stable internet connection.

> **Back up your USB drive first.** Fedora Media Writer, Rufus, Ventoy, and
> similar tools can erase or repartition the selected drive.

## Installation

### Step 1: Choose the correct image

| Image | Use it when |
|---|---|
| Fedora Workstation 45, x86_64 | You have a normal Intel or AMD laptop or desktop. |
| Fedora Workstation 45, aarch64 | You have an ARM device that supports Fedora Workstation. |
| Fedora KDE Plasma Desktop | You want KDE Plasma instead of GNOME. |
| Fedora Server | You are setting up a server without a desktop. |

This tutorial is for **Fedora Workstation with GNOME**. If you are unsure, choose
**Fedora Workstation 45, x86_64**.

### Step 2: Download from the official website

1. Open the official
   [Fedora Workstation download page](https://fedoraproject.org/workstation/download/).
2. Confirm that the page shows **Fedora Workstation 45**.
3. Choose the **x86_64** image for Intel and AMD computers, or **aarch64** for
   ARM devices.
4. Download the Live ISO. The file name looks like:
   ```text
   Fedora-Workstation-Live-45-1.4.x86_64.iso
   ```
5. Also download the matching `-CHECKSUM` file and the Fedora OpenPGP key from
   the same download page.

Do not download Fedora from an unofficial mirror or a random download site.

### Step 3: Verify the download

Verification checks two things:

- the file was not damaged during download (**checksum**)
- the file was published by the Fedora Project (**OpenPGP signature**)

#### Check the checksum

Open a terminal in the folder that contains the ISO and checksum file.

**Linux:**

```bash
sha256sum Fedora-Workstation-Live-45-1.4.x86_64.iso
```

**Windows PowerShell:**

```powershell
Get-FileHash .\Fedora-Workstation-Live-45-1.4.x86_64.iso -Algorithm SHA256
```

Compare the result with the SHA-256 value in the official `-CHECKSUM` file. The
long hexadecimal strings must match.

If they differ, delete the ISO and download it again.

#### Verify the OpenPGP signature

This is the stronger check. It confirms that the checksum file was signed by the
Fedora Project.

Download the Fedora keyring and verify the checksum file:

```bash
curl -O https://fedoraproject.org/fedora.pgp
gpgv --keyring ./fedora.pgp --output - Fedora-Workstation-45-1.4-x86_64-CHECKSUM | sha256sum -c --ignore-missing
```

- `curl -O` downloads the Fedora OpenPGP keyring.
- `gpgv` checks the signature on the checksum file.
- `sha256sum -c` checks the ISO against the verified checksum.

Use the exact checksum file name shown on the download page. The build number
changes between releases and respins.

A successful result includes the ISO file name followed by `OK`. If the
signature fails, do not use the image. Download it again from the official site.

**Official reference:**
[Verifying Fedora Linux downloads](https://docs.fedoraproject.org/en-US/security/cryptography/signatures/)

### Step 4: Write the image with Fedora Media Writer

1. Download Fedora Media Writer from the official
   [Fedora Workstation download page](https://fedoraproject.org/workstation/download/).
2. Install and open Fedora Media Writer.
3. Choose **Select .iso file** if you already downloaded the ISO, or let Media
   Writer download Fedora for you.
4. Insert the USB drive.
5. Select the correct drive. Verify its size and name.
6. Choose **Write** and read the warning about erasing the drive.
7. Wait until the write completes.
8. Safely eject the USB drive.

Fedora Media Writer is the recommended tool because it handles Fedora images and
verifies the download. If you prefer another tool, follow
[Create a bootable USB drive](../guides/bootable-usb.md).

> **Disk safety:** Select the USB drive, not an internal disk. Writing an image
> to the wrong device destroys data. Check the drive's name and capacity before
> you confirm.

### Step 5: Keep the installation media

Keep the USB drive until Fedora is installed and you have confirmed that the
system boots without it. You may also want it for recovery later.

## Configuration

You do not need to configure Fedora at this stage. Record two choices:

- **Architecture:** `x86_64` for most PCs, `aarch64` for ARM devices.
- **Edition:** Workstation for GNOME, KDE Plasma Desktop for KDE, or Server for a
  server install.

If you plan to keep Windows, read
[Dual-booting Linux and Windows](../guides/dual-boot.md) before you start the
installer. Back up your files and save the BitLocker recovery key if Windows uses
BitLocker.

### Secure Boot

Fedora supports Secure Boot on compatible hardware. Do not disable Secure Boot as
a first step. If the USB does not boot, check the firmware settings and Fedora's
official installation documentation before changing security settings.

## Verification

Before installing, confirm:

- [ ] The ISO came from the official Fedora website.
- [ ] The SHA-256 checksum matches the official value.
- [ ] The OpenPGP signature on the checksum file is valid.
- [ ] Fedora Media Writer reported a successful write.
- [ ] The computer can see the USB drive in its boot menu.

The final check is to boot the USB and choose the live session without installing.
See [Create a bootable USB drive](../guides/bootable-usb.md#verification).

## Troubleshooting

### The download stops or is very slow

- Try the official download page again at a different time.
- Use a wired connection if possible.
- Do not use an unofficial mirror unless the official Fedora page links to it.

### The checksum does not match

- Download the ISO again.
- Confirm that you downloaded the matching `-CHECKSUM` file for the same build.
- Check that the file name in the command matches your actual file.
- Do not install from an ISO that fails verification.

### The OpenPGP signature fails

- Confirm that you downloaded `fedora.pgp` from `fedoraproject.org`.
- Confirm that the checksum file is the one published for your exact ISO.
- Download both files again.
- Do not bypass signature verification.

### Fedora Media Writer cannot detect the USB drive

- Reconnect the drive and try another USB port.
- Close programs that may be using the drive.
- Confirm that the drive is not write-protected.
- Try another USB drive.
- On Linux, check that the drive is not mounted by another application.

### The computer does not boot from the USB

- Open the one-time boot menu. Common keys are `F12`, `F11`, `Esc`, or `F9`, but
  the key depends on the manufacturer.
- Check whether the computer is in UEFI or legacy mode.
- Recreate the USB with Fedora Media Writer.
- Do not disable Secure Boot as a first step.

### I downloaded the wrong architecture

The ISO will not boot on an incompatible CPU. Check your computer's architecture
before downloading. Most Intel and AMD systems use `x86_64`; many ARM devices use
`aarch64`.

## Uninstall or Restore

This page only downloads a file and writes a USB drive. To undo it:

- Delete the ISO and checksum files if you no longer need them. This frees about
  3 GB.
- Restore the USB drive with your operating system's disk-management tool.
  **Formatting the USB erases everything on it.**
- If you no longer need the Fedora keyring file, delete `fedora.pgp` from the
  download folder.

## References

- [Fedora Workstation download](https://fedoraproject.org/workstation/download/)
- [Fedora Media Writer](https://fedoraproject.org/workstation/download/)
- [Preparing boot media](https://docs.fedoraproject.org/en-US/quick-docs/creating-and-using-a-live-installation-image/)
- [Verifying Fedora Linux downloads](https://docs.fedoraproject.org/en-US/security/cryptography/signatures/)
- [Fedora 45 release notes](https://docs.fedoraproject.org/en-US/release-notes/f45/)
- [Create a bootable USB drive](../guides/bootable-usb.md)

**Next:** [Install Fedora](./03-installation.md)
