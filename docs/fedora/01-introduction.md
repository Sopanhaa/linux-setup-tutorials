# Introduction — Fedora

**Supported version:** Fedora Workstation 45 (current release at the time of
writing). Most steps also apply to Fedora Workstation 44.

**Difficulty:** Beginner

**Terminal required:** No for this page. The example commands are optional.

**Last reviewed:** Not yet reviewed on live systems.

**Learning path:** [Fedora overview](./README.md) · [Download](./02-download.md) →

## Overview

Fedora Workstation is a Linux distribution for laptops and desktop computers. It
is built by the Fedora Project and ships with the GNOME desktop, current Linux
kernels, and a strong focus on free and open-source software.

This page explains what Fedora is, who it is for, what hardware it needs, and how
its releases work. It does not install anything. Installation starts on the
[Download](./02-download.md) page.

## What is Fedora?

Fedora is a **Linux distribution**. It combines:

- the Linux kernel
- system tools and libraries
- the DNF package manager
- the GNOME desktop environment on Fedora Workstation
- common applications such as Firefox and LibreOffice
- security tools such as SELinux and firewalld

Fedora is free to download, install, and use. The Fedora Project is community-run
and sponsored by Red Hat.

### Fedora, Red Hat, and RHEL

Fedora is not the same as Red Hat Enterprise Linux (RHEL).

| Name | What it is |
|---|---|
| Fedora | A fast-moving community Linux distribution sponsored by Red Hat |
| RHEL | Red Hat's commercial, long-support enterprise Linux distribution |
| CentOS Stream | A continuously updated stream that sits between Fedora and RHEL |

Fedora often introduces new Linux technology first. RHEL later adopts some of
that work with longer support periods. Fedora is a good desktop for people who
want current software and are comfortable upgrading regularly.

## Who should use Fedora Workstation?

Fedora Workstation is a good fit if you:

- want a clean GNOME desktop with current software
- develop software or work with containers
- want a distribution with strong security defaults
- are comfortable upgrading to a new release about once a year
- prefer free and open-source software

Fedora may not be the best choice if you:

- need a release with five or more years of support
- want to avoid frequent major upgrades
- depend on software that only supports Ubuntu or Windows
- need a very small or very old computer with minimal resources

There is no single best Linux distribution. Fedora is a strong choice for a
modern desktop, especially for developers.

## System requirements

Fedora publishes both minimum and recommended requirements. The figures below
come from the Fedora 45 release notes.

| Component | Minimum | Recommended |
|---|---|---|
| Processor | 2 GHz dual-core | 2 GHz quad-core |
| Memory | 4 GB RAM | 8 GB RAM |
| Storage | 15 GB free space | 20 GB free space |
| Display | 1024 × 768 for the graphical installer | Higher resolution for daily use |
| Installation media | USB drive, usually 4 GB or larger | 8 GB or larger |

The [Fedora Workstation documentation](https://docs.fedoraproject.org/en-US/workstation-docs/)
suggests a 40 GB SSD and 4 GB RAM for a good experience. Those numbers are older
and less precise than the release notes. For a comfortable desktop in 2026,
treat **8 GB RAM and 40 GB of free disk space** as a practical baseline.

Fedora Workstation images are available for:

- **x86_64** — most Intel and AMD laptops and desktops
- **aarch64** — ARM devices such as some laptops and single-board computers

> **Before you install:** back up important files. Installing an operating
> system can erase a disk if you choose the wrong option.

## Editions, Atomic Desktops, and Spins

Fedora is available in several forms. This tutorial covers **Fedora Workstation
with GNOME**.

| Edition or variant | Desktop or purpose |
|---|---|
| Fedora Workstation | GNOME desktop for general desktop use and development |
| Fedora KDE Plasma Desktop | KDE Plasma desktop edition |
| Fedora Server | Server installation without a desktop by default |
| Fedora Atomic Desktops | Immutable desktops such as Silverblue and Kinoite |
| Fedora Spins | Alternative desktops such as Xfce, Cinnamon, MATE, and LXQt |
| Fedora Labs | Purpose-built collections such as Design Suite and Security Lab |

Do not follow Atomic Desktop commands on a normal Workstation system. Atomic
desktops use `rpm-ostree` instead of DNF for system packages. This tutorial is
for the traditional Fedora Workstation edition.

## Release cycle and support

Fedora releases a new version about every six months.

- Each release usually receives updates for about **13 months**.
- Fedora does not offer a long-term support (LTS) release.
- You should upgrade before your release reaches end of life.
- Fedora officially tests upgrades across **at most two releases**. Do not skip
  several releases at once.

Fedora 45 was released on **November 3, 2026**, with GNOME 51. Fedora 44 remains
supported for a while after that. Plan to upgrade to a newer release each year.

## What is included in Fedora Workstation 45?

Fedora Workstation 45 includes:

- **GNOME 51** with a Wayland session
- **Linux kernel 7.2**
- **DNF5**, the current DNF package manager
- **Flatpak** support
- **PipeWire** for audio and video routing
- **Btrfs** with separate `/` and `/home` subvolumes by default
- **firewalld** for firewall management
- **SELinux** in enforcing mode by default
- **Secure Boot** support on compatible hardware
- Firefox, LibreOffice, and other desktop applications

Fedora 45 also enforces RPM package signature checking by default. Do not
disable signature checks to install software. Use packages from trusted
repositories.

## Try Fedora before installing

You can test Fedora without changing your disk.

1. Download the Fedora Workstation Live ISO.
2. [Create a bootable USB drive](../guides/bootable-usb.md) or use Fedora Media
   Writer.
3. Restart the computer and open the boot menu.
4. Choose the USB drive.
5. Select **Start Fedora Workstation Live**.
6. Test Wi-Fi, sound, display, keyboard, and touchpad.
7. Shut down and remove the USB when finished.

The live session runs from the USB drive. It is slower than an installed system
and should not be used for important work.

## Practical examples for beginners

Open **Terminal** from the application menu, or press `Ctrl` + `Alt` + `T` if
your desktop supports that shortcut.

Commands are case-sensitive. Type a command and press `Enter`.

### Check your Fedora version

```bash
cat /etc/os-release
```

You will see lines like:

```text
NAME="Fedora Linux"
VERSION="45 (Workstation Edition)"
```

### Check your hardware

```bash
nproc
free -h
df -h /
```

- `nproc` shows the number of CPU threads.
- `free -h` shows memory in human-readable units.
- `df -h /` shows free space on the root filesystem.

These commands only display information.

### Check your desktop session

```bash
echo "$XDG_CURRENT_DESKTOP"
echo "$XDG_SESSION_TYPE"
```

The first command usually prints `GNOME`. The second usually prints `wayland` on
Fedora Workstation 45.

### Check DNF

```bash
dnf --version
```

Fedora 45 uses DNF5. The output shows the DNF version and confirms that the
`dnf` command is available.

### Check security services

```bash
getenforce
sudo firewall-cmd --state
```

- `getenforce` should print `Enforcing` on a normal Fedora Workstation install.
- `firewall-cmd --state` should print `running`.

`sudo` runs the firewall check with administrator privileges. The command only
reads the firewall state.

> **Safety note:** Do not disable SELinux or firewalld as a first troubleshooting
> step. Fix the specific problem instead.

## Next steps

Continue with the Fedora learning path:

1. [Download Fedora](./02-download.md) and verify the image.
2. [Create a bootable USB drive](../guides/bootable-usb.md).
3. [Install Fedora](./03-installation.md).
4. [Complete the initial setup](./04-initial-setup.md).
5. [Set up your desktop](./05-desktop-setup.md).
6. [Troubleshoot common problems](./06-troubleshooting.md).

Useful companion guides:

- [Package managers](../guides/package-managers.md)
- [Graphics drivers](../guides/graphics-drivers.md)
- [Basic Linux commands](../guides/basic-commands.md)
- [Backup and restore](../maintenance/backup-and-restore.md)

## References

- [Fedora Workstation](https://fedoraproject.org/workstation/)
- [Fedora Workstation documentation](https://docs.fedoraproject.org/en-US/workstation-docs/)
- [Fedora 45 release notes](https://docs.fedoraproject.org/en-US/release-notes/f45/)
- [Fedora 45 announcement](https://fedoramagazine.org/announcing-fedora-linux-45/)
- [Fedora documentation](https://docs.fedoraproject.org/)
- [Fedora Discussion](https://discussion.fedoraproject.org/)
- [Ask Fedora](https://ask.fedoraproject.org/)

**Next:** [Download Fedora](./02-download.md)
