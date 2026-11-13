# Create a Bootable USB Drive

**Supported systems:** [Windows](https://www.microsoft.com/windows) and [Linux](https://www.kernel.org/) (depending on the tool)   

**General guides:** [All guides](./README.md)

A **bootable USB** lets you try, install, or repair Linux on your computer.

You can use it to:

- **Try Linux**: Test Linux without installing it.
- **Install Linux**: Install Ubuntu, Fedora, Arch Linux, or other distributions.
- **Repair Linux**: Fix system problems using recovery tools.

### What is an ISO File?

An **ISO file** contains the files needed to start or install an operating system.

Example: `ubuntu-26.04-desktop-amd64.iso`

Copying an ISO file to a USB drive normally does not make it bootable. Use a tool like **[Rufus](https://rufus.ie/)** or **[Ventoy](https://www.ventoy.net/)** to create a bootable USB.

### Choose a Method

| Method | Platform | Description |
|--------|----------|-------------|
| **[Rufus](https://rufus.ie/)** | Windows | Create a bootable USB with one Linux ISO. |
| **[Ventoy](https://www.ventoy.net/)** | Windows and Linux | Use multiple Linux ISO files on one USB. |

**Which should you use?**

- **[Rufus](https://rufus.ie/)**: Best for installing one Linux distribution on Windows.
- **[Ventoy](https://www.ventoy.net/)**: Best for testing multiple Linux distributions on Windows or Linux.

## Requirements

Before you begin, make sure you have:

- **Computer**: Windows or Linux with internet access.
- **USB drive**: At least 8 GB of storage (or enough for your ISO).
- **Linux ISO**: Download from an official website such as [Ubuntu](https://ubuntu.com/download), [Fedora](https://fedoraproject.org/), or [Arch Linux](https://archlinux.org/download/).
- **USB tool**: [Rufus](https://rufus.ie/) or [Ventoy](https://www.ventoy.net/).
- **Backup**: Save important files from your USB before starting.

## Step 1: Download a Linux ISO

Choose a distribution and download its installation image from its official website.

| Distribution | Official download |
| --- | --- |
| Ubuntu | [Ubuntu Desktop](https://ubuntu.com/download/desktop) |
| Fedora | [Fedora Workstation](https://fedoraproject.org/workstation/download/) |
| Arch Linux | [Arch Linux Downloads](https://archlinux.org/download/) |
| Debian | [Debian Downloads](https://www.debian.org/distrib/) |
| Linux Mint | [Linux Mint Downloads](https://linuxmint.com/download.php) |
| openSUSE | [openSUSE Downloads](https://get.opensuse.org/) |

1. Open your distribution's official download page.
2. Choose the edition and CPU architecture suitable for your computer.
3. Download the ISO file and remember where you saved it.
4. Find the official checksum or verification instructions on the same distribution website.

### Check ISO download

A **SHA-256 checksum** helps detect incomplete or changed downloads. Replace `filename.iso` with your file's actual name and run the appropriate command in the directory containing the ISO.

**Windows (PowerShell):**

```powershell
Get-FileHash .\filename.iso -Algorithm SHA256
```

**Linux (terminal):**

```bash
sha256sum filename.iso
```

**macOS (Terminal, for checking a downloaded ISO):**

```bash
shasum -a 256 filename.iso
```

Compare the result with the SHA-256 value published **by the distribution**. If it differs, do not use the ISO; download it again. A checksum comparison verifies file integrity against the published value. When the distribution offers a cryptographic signature, follow its official signature-verification guide for authenticity as well.

See [Arch Linux Downloads](https://archlinux.org/download/) or [Linux Mint ISO Verification](https://linuxmint-installation-guide.readthedocs.io/en/latest/verify.html) for examples of distribution-specific verification guidance.

## Step 2: Choose a USB creation tool

| Tool | Operating system | Good for | Official website |
| --- | --- | --- | --- |
| Rufus | Windows | Creating a bootable USB from one ISO | [rufus.ie](https://rufus.ie/) |
| Ventoy | Windows, Linux | Keeping multiple supported ISO images on one USB | [ventoy.net](https://www.ventoy.net/) |

### Rufus (Windows)

Rufus writes a Linux installation image to a USB drive. Follow the distribution's own Rufus instructions if they specify different settings.

1. Download Rufus from [rufus.ie](https://rufus.ie/) and open it on Windows.
2. Insert your USB drive.
3. Under **Device**, select the USB drive. Verify its name and capacity carefully.
4. Under **Boot selection**, choose **SELECT** and open your downloaded `.iso` file.
5. Keep the recommended settings unless your distribution's official documentation says otherwise. Partition scheme and image-writing mode can depend on the ISO and your computer's firmware.
6. Click **START**. Read any prompts about ISO/DD image mode or additional files, and follow your distribution's guidance.
7. Read the warning about erasing the selected device. Proceed only after confirming the correct USB.
8. Wait until Rufus reports **READY**, then safely eject the drive.

**Next:** [Verify the USB boots](#step-3-verify-the-usb-boots).

Official reference: [Rufus](https://rufus.ie/) · [Ubuntu's bootable USB guide](https://ubuntu.com/desktop/docs/en/latest/how-to/create-a-bootable-usb-stick/)

### Ventoy (Windows and Linux)

Ventoy lets you keep multiple ISO files on a USB drive. Installing Ventoy **onto the drive** can erase or change its partitions; later, supported ISO files are normally copied onto the Ventoy data partition.

#### Install Ventoy on Windows

1. Download the Windows package from [Ventoy Downloads](https://www.ventoy.net/en/download.html).
2. Extract it and open `Ventoy2Disk.exe`.
3. Select your USB drive. Confirm its name and capacity.
4. Choose **Install** and read the erase/repartition warnings.
5. Proceed only if the selected drive is correct. Wait until installation finishes.

#### Install Ventoy on Linux

1. Download the Linux package from [Ventoy Downloads](https://www.ventoy.net/en/download.html) and extract it.
2. Follow the official [Ventoy Linux GUI instructions](https://www.ventoy.net/en/doc_linux_gui.html) or [Ventoy getting-started guide](https://www.ventoy.net/en/doc_start.html).
3. Use the provided GUI to select your **USB device**, then verify the device name and size before installing.
4. Review the disk-erasure warning carefully, then complete the installation.

> **Disk safety:** Linux disk names such as `/dev/sdb` are examples, not reliable identifiers for your USB. Do not run a disk-writing command with a copied device name. Choosing the wrong disk can destroy data on your computer.

#### Add and boot ISO files

1. Open the main Ventoy data partition on the USB drive.
2. Copy one or more **supported** Linux ISO files to it. Do not extract the ISO files.
3. Wait for copying to complete and safely eject the USB.
4. Restart the computer, open its boot menu, and select the USB drive.
5. In Ventoy's menu, select the ISO you want to boot.

Ventoy compatibility varies by ISO, firmware, and Secure Boot configuration. Check its [tested ISO list](https://www.ventoy.net/en/isolist.html) and your distribution's installation notes. If a distribution's installer fails through Ventoy, try its recommended direct-image writing method instead.

Official references: [Ventoy getting started](https://www.ventoy.net/en/doc_start.html) · [Ventoy documentation](https://www.ventoy.net/en/doc_start.html)

## Step 3: Verify the USB boots

A successful write message does **not** prove the USB will boot on every computer. Test it before relying on it for installation.

1. Keep the USB connected and restart your computer.
2. Open its **one-time boot menu**. Common keys include `F12`, `F11`, `Esc`, or `F9`, but the actual key depends on your device. Check the manufacturer's documentation.
3. Choose the USB device from the boot menu.
4. Confirm that the Linux live environment, installer menu, or Ventoy menu appears.
5. If possible, start the **live** or **try** mode without installing anything.
6. Shut down or restart normally after testing. You do **not** have to install Linux to test the USB.

### Verification checklist

- [ ] ISO downloaded from the official distribution website.
- [ ] ISO checksum matched the official published checksum.
- [ ] Correct USB device selected, and any important files backed up.
- [ ] USB-writing tool reported completion.
- [ ] Computer detected the USB in its boot menu.
- [ ] Expected Linux boot or live environment started.

## Troubleshooting

### USB not listed in the boot menu

- Reconnect the USB drive and try a different USB port.
- Make sure you opened the **boot menu**, not only the firmware settings screen.
- Recreate the USB with the tool recommended by your distribution.
- Check the computer manufacturer's boot instructions.

### Linux boot screen does not appear

- Verify the ISO checksum again.
- Check whether the Linux image supports your computer's CPU architecture and firmware mode.
- Try a different USB drive or writing tool.
- Consult the distribution's official UEFI/Secure Boot documentation.

> **Secure Boot:** Do not disable Secure Boot as a first troubleshooting step. Check whether your distribution and boot method support it. Changing firmware security settings can affect Windows startup or BitLocker/device encryption; know the recovery requirements first.

### Rufus or Ventoy cannot write to the USB

- Close applications that might be using the USB drive.
- Unplug and reconnect it, then verify the selected device.
- Try another USB port or known-working USB drive.
- Follow the tool's official troubleshooting documentation.

### Ventoy starts, but the ISO does not

- Ensure the ISO was copied completely to Ventoy's data partition.
- Confirm the ISO is supported by your Ventoy version.
- Read the distribution's notes about Ventoy compatibility.
- If necessary, write the ISO directly with a distribution-recommended method.

### USB appears smaller afterward

Writing an ISO or installing Ventoy can change the partition layout. After you no longer need the bootable media, restore the USB through your operating system's disk-management tool. **Formatting or repartitioning erases data**; carefully confirm which drive you are changing.

## Next steps

Follow your chosen distribution's installation tutorial:

- [Ubuntu](../ubuntu/README.md)
- [Fedora](../fedora/README.md)
- [Arch Linux](../arch-linux/README.md)
- [Debian](../debian/README.md)
- [Linux Mint](../linux-mint/README.md)
- [openSUSE](../opensuse/README.md)

You can also [browse all general Linux guides](./README.md).

## Official references

- [Ubuntu Create a bootable USB stick](https://ubuntu.com/desktop/docs/en/latest/how-to/create-a-bootable-usb-stick/)
- [Rufus Official website](https://rufus.ie/)
- [Ventoy Getting started](https://www.ventoy.net/en/doc_start.html)
- [ArchWiki USB flash installation medium](https://wiki.archlinux.org/title/USB_flash_installation_medium)
- [Linux Mint Verify your ISO](https://linuxmint-installation-guide.readthedocs.io/en/latest/verify.html)


