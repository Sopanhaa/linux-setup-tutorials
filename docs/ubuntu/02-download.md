# Download — Ubuntu

**Supported version:** Ubuntu 26.04 LTS Desktop (amd64) and Ubuntu 24.04 LTS
Desktop (amd64)

**Difficulty:** Beginner

**Terminal required:** No, but the checksum step uses a terminal or PowerShell.

**Last reviewed:** Not yet reviewed on live systems.

**Learning path:** ← [Introduction](./01-introduction.md) · [Ubuntu overview](./README.md) · [Installation](./03-installation.md) →

## Overview

This tutorial helps you download the correct Ubuntu Desktop image, check that
the file is complete, and write it to a USB drive.

By the end, you will have installation media that can boot your computer. You
will not install Ubuntu yet. That is the next page:
[Installation](./03-installation.md).

For the USB-writing steps, this tutorial links to the project's
[Create a bootable USB drive](../guides/bootable-usb.md) guide instead of
repeating the same instructions.

## Requirements

- A computer with internet access.
- A USB drive with at least 8 GB of free space. **Writing the image erases the
  USB drive.**
- A stable connection if your download is large.
- Enough free disk space for the ISO file (usually 4–8 GB).

> **Back up your USB drive first.** Rufus, Ventoy, and similar tools can erase
> or repartition the selected drive.

## Installation

### Step 1: Choose the correct edition

| Edition | Use it when |
|---|---|
| Ubuntu Desktop 26.04 LTS | You want the current long-term support release for a normal desktop or laptop. |
| Ubuntu Desktop 24.04 LTS | You need the previous LTS release, for example because a tool supports it. |
| Ubuntu Server | You are setting up a server without a graphical desktop. |

Most laptops and desktop PCs use the **64-bit Intel/AMD** image, also called
**amd64**. Use **arm64** only for ARM devices such as some Raspberry Pi boards or
ARM laptops.

If you are unsure, choose **Ubuntu Desktop 26.04 LTS, amd64**.

### Step 2: Download from the official website

1. Open the official [Ubuntu Desktop download page](https://ubuntu.com/download/desktop).
2. Choose **Ubuntu 26.04 LTS** or **Ubuntu 24.04 LTS**.
3. Choose the **amd64** image for a normal PC.
4. Download the ISO file. Do not download it from a mirror or a random download
   site.
5. Note the folder where the file was saved.

The file name looks similar to:

```text
ubuntu-26.04-desktop-amd64.iso
```

### Step 3: Verify the download

A checksum is a short fingerprint of a file. If the checksum of your download
matches the official value, the file was not damaged during download.

Ubuntu publishes checksums and signature files on its official release archive
([releases.ubuntu.com](https://releases.ubuntu.com/)). Download `SHA256SUMS` and
`SHA256SUMS.gpg` from the same release folder as the ISO, then open a terminal in
the folder that contains the ISO.

**Ubuntu and other Linux systems:**

```bash
sha256sum ubuntu-26.04-desktop-amd64.iso
```

**Windows PowerShell:**

```powershell
Get-FileHash .\ubuntu-26.04-desktop-amd64.iso -Algorithm SHA256
```

**macOS:**

```bash
shasum -a 256 ubuntu-26.04-desktop-amd64.iso
```

Compare the output with the value in the official `SHA256SUMS` file. The long
hexadecimal strings must match exactly.

- If they match, the download is complete.
- If they differ, delete the ISO and download it again.

A checksum confirms file integrity, not who published the file. For authenticity,
follow Ubuntu's official instructions for verifying the GPG signature of
`SHA256SUMS`.

**Official reference:** [Verify your Ubuntu download](https://ubuntu.com/tutorials/how-to-verify-ubuntu)

### Step 4: Write the ISO to a USB drive

Follow [Create a bootable USB drive](../guides/bootable-usb.md). Use Rufus on
Windows or Ventoy on Windows or Linux.

> **Disk safety:** Select the correct USB device. Writing an ISO to the wrong
> disk destroys data. Check the drive's name and capacity before you confirm.

### Step 5: Keep the installation media

Keep the USB drive until Ubuntu is installed and you have confirmed that the
system boots without it. You may also want it for recovery later.

## Configuration

You do not need to configure Ubuntu at this stage. Two choices are worth
recording:

- **Which version you downloaded:** LTS releases receive five years of standard
  support. Interim releases receive nine months.
- **Which architecture you downloaded:** `amd64` for most PCs, `arm64` for ARM
  devices.

If you plan to keep Windows, read
[Dual-booting Linux and Windows](../guides/dual-boot.md) before you start the
installer. Back up your files and note whether Windows uses BitLocker.

## Verification

Before installing, confirm:

- [ ] The ISO came from the official Ubuntu website.
- [ ] The SHA-256 checksum matches the official value.
- [ ] The USB drive contains the Ubuntu image, not just a copied ISO file.
- [ ] The computer can see the USB drive in its boot menu.

The final check is to boot the USB and choose **Try Ubuntu** without installing.
See [Create a bootable USB drive](../guides/bootable-usb.md#verification).

## Troubleshooting

### The download stops or is very slow

- Try the official download page again at a different time.
- Use a wired connection if possible.
- Do not use an unofficial mirror unless Ubuntu's download page links to it.

### The checksum does not match

- Download the ISO again.
- Confirm that you downloaded the matching `SHA256SUMS` file for the same
  release.
- Check that the file name in the command matches your actual file.
- Do not install from an ISO that fails verification.

### The USB is not detected by Rufus or Ventoy

- Reconnect the USB drive and try another port.
- Close programs that may be using the drive.
- Confirm that the drive is not write-protected.
- Try another USB drive.

### The computer does not boot from the USB

- Open the one-time boot menu. Common keys are `F12`, `F11`, `Esc`, or `F9`, but
  the key depends on the manufacturer.
- Check whether the computer is in UEFI or legacy mode.
- Recreate the USB with the tool recommended by Ubuntu.
- Do not disable Secure Boot as a first step. Ubuntu supports Secure Boot on
  supported hardware; check the official release notes for your version.

## Uninstall or Restore

This page only downloads a file and writes a USB drive. To undo it:

- Delete the ISO file if you no longer need it. This frees several gigabytes.
- Restore the USB drive to normal storage with your operating system's disk
  management tool. **Formatting the USB erases everything on it.**
- If you created a Ventoy drive, reinstall Ventoy or reformat the drive to remove
  its partition layout.

## References

- [Ubuntu Desktop download](https://ubuntu.com/download/desktop)
- [Ubuntu release cycle](https://ubuntu.com/about/release-cycle)
- [Verify your Ubuntu download](https://ubuntu.com/tutorials/how-to-verify-ubuntu)
- [Create a bootable USB stick](https://ubuntu.com/desktop/docs/en/latest/how-to/create-a-bootable-usb-stick/)
- [Ubuntu documentation](https://documentation.ubuntu.com/)

**Next:** [Install Ubuntu](./03-installation.md)
