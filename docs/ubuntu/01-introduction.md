# Introduction — Ubuntu

**Learning path:** [Ubuntu overview](./README.md) · [Download](./02-download.md) →

Ubuntu is a free, open-source operating system and one of the most popular Linux distributions. It is a common first choice for people moving away from Windows or macOS.

This page explains what Ubuntu is, who it is for, what hardware it needs, and which version you should choose.

## In this tutorial

- What Ubuntu is
- How Ubuntu relates to Linux and Debian
- Who should use Ubuntu
- Minimum and recommended system requirements
- Ubuntu Desktop, Ubuntu Server, and official flavours
- LTS releases, interim releases, and support periods
- Practical examples for Linux beginners
- Official documentation and download links

## What is Ubuntu?

Ubuntu is a **Linux distribution** (often called a **distro**) published by **Canonical**.

An operating system manages your computer's hardware and runs your applications. Ubuntu includes:

- the Linux kernel
- system tools and libraries
- a package manager for installing software
- a desktop environment
- common applications

Ubuntu is free to download, install, and use. Canonical earns money from support, management tools, and services such as Ubuntu Pro. You do not have to pay to use the basic operating system.

The name *Ubuntu* comes from an African word meaning "humanity to others". The first Ubuntu release, 4.10, arrived in October 2004.

## How Ubuntu relates to Linux and Debian

These three names are related, but they are not the same thing.

### Linux is the kernel

Linux is not a complete operating system by itself. **Linux is the kernel**: the core part of the system that talks to your hardware and manages memory, processes, and devices. Linus Torvalds started the Linux kernel in 1991.

A Linux distribution combines the kernel with other software:

> Linux kernel + system tools + package manager + desktop + applications = a Linux distribution

Different distributions make different choices about that software. That is why Ubuntu, Fedora, Debian, and Arch Linux look and behave differently, even though they all use the Linux kernel.

### Debian is Ubuntu's base

**Debian** is a long-running, community-driven Linux distribution. It is known for stability and for its very large collection of software packages.

Ubuntu began in 2004. Mark Shuttleworth gathered a small team of Debian developers and founded Canonical to build it. Ubuntu uses Debian's package format (`.deb`) and the `apt` package manager. Many Ubuntu packages also come from Debian.

Ubuntu is not simply Debian with a new name. It has its own:

- release schedule and support policy
- kernel and default software choices
- installer and desktop experience
- security process and commercial support
- Snap package support

In short: **Ubuntu is a Linux distribution built on Debian.**

### Quick comparison

| Name | What it is |
|---|---|
| Linux | The kernel at the core of the system |
| Debian | A community-run Linux distribution and Ubuntu's main base |
| Ubuntu | A Linux distribution based on Debian, published by Canonical |

## Who should use Ubuntu?

Ubuntu is a good fit if you:

- are using Linux for the first time
- are a student, teacher, or researcher
- write software or work with servers
- want a stable computer for office and everyday tasks
- need long-term security updates for a business or organisation
- want a well-supported system for cloud, containers, or development
- have an older computer and can use a lighter flavour such as Lubuntu or Xubuntu

Ubuntu may not be the best fit if you:

- want the newest kernel and applications every few weeks
- prefer a rolling-release distribution such as Arch Linux or openSUSE Tumbleweed
- want a distribution governed only by its community, without a commercial publisher
- depend on professional software that runs only on Windows or macOS
- have very limited memory or storage and need the lightest possible system

There is no single "best" Linux distribution. Ubuntu is a strong default for beginners because it has a large community, clear documentation, and wide hardware support. If you prefer a purely community-run system, see the [Debian overview](../debian/README.md).

## System requirements

The figures below are the official minimum requirements for **Ubuntu 26.04 LTS**. Requirements can change between releases, so check the official page for the version you plan to install.

### Ubuntu Desktop 26.04 LTS

| Component | Minimum |
|---|---|
| Processor | 2 GHz dual-core processor or better |
| Memory | 6 GB RAM |
| Storage | 25 GB of free disk space |
| Installation media | A USB port or DVD drive |
| Internet | Helpful, but not required for the initial installation |

Ubuntu Desktop can install on some systems with lower specifications. However, the experience may be slow. For a computer with 2 GB of RAM, the official release notes suggest a lighter flavour such as [Lubuntu](https://lubuntu.me/) or [Xubuntu](https://xubuntu.org/).

### Ubuntu Server 26.04 LTS

| Component | Minimum |
|---|---|
| Memory | 1.5 GB RAM for an ISO install |
| Storage | 5 GB of free disk space for an ISO install |
| Installation media | A USB port or DVD drive |

Server requirements depend heavily on what you run. The official Server documentation suggests at least **3 GB of RAM** and **25 GB of storage** for more comfortable setups. Busy servers, databases, and virtual machines need more.

### Recommended for a comfortable desktop

These values are practical suggestions from this tutorial, not official minimums:

| Component | Recommendation |
|---|---|
| Processor | 4 cores or more |
| Memory | 8 GB RAM or more |
| Storage | 40 GB or more on an SSD |
| Internet | A reliable connection for updates |

### Which image should you download?

Most laptops and desktop PCs need the **64-bit Intel/AMD** image, also called **amd64**.

Use the **ARM 64-bit** image (**arm64**) for devices such as Raspberry Pi boards or ARM-based laptops. Your download page will show the available options.

> **Before you install:** back up important files. Installing an operating system can erase a disk if you choose the wrong options.

## Ubuntu Desktop, Ubuntu Server, and official flavours

Ubuntu comes in several editions. They share the same core system but are built for different jobs.

### Ubuntu Desktop

Ubuntu Desktop is the standard version for laptops and desktop computers. It includes:

- the GNOME desktop environment
- a graphical installer
- a web browser and common desktop applications
- a live session that lets you try Ubuntu before installing

This is the right choice for most home users, students, and developers who want a normal desktop computer.

**Download:** [Ubuntu Desktop](https://ubuntu.com/download/desktop)

### Ubuntu Server

Ubuntu Server is designed for servers, cloud instances, and containers. It:

- installs without a graphical desktop by default
- uses a text-based installer
- focuses on services, networking, and remote access
- is available for amd64, arm64, and other server architectures

You can add a desktop later, but most servers are managed from the command line or over SSH. Most server users should choose an LTS release.

**Download:** [Ubuntu Server](https://ubuntu.com/download/server)

### Other official editions

Ubuntu also provides:

- **Ubuntu Core** — a minimal, snap-based system for appliances and IoT devices
- **Ubuntu on WSL** — Ubuntu running inside Windows Subsystem for Linux
- **Cloud images** — prebuilt images for providers such as AWS, Azure, and Google Cloud

These editions are useful, but they are not covered by this beginner tutorial.

### Official Ubuntu flavours

Ubuntu **flavours** are official variants maintained by members of the Ubuntu community. Each one offers a different desktop environment or a special focus, but they all use the Ubuntu package archive.

| Flavour | Focus | Best for |
|---|---|---|
| Edubuntu | Education | Schools, students, and teachers |
| Kubuntu | KDE Plasma desktop | Users who want a highly customisable desktop |
| Lubuntu | LXQt desktop | Older or low-resource computers |
| Ubuntu Budgie | Budgie desktop | A simple, modern desktop |
| Ubuntu Cinnamon | Cinnamon desktop | A traditional desktop layout |
| Ubuntu Kylin | UKUI desktop | Chinese-language users and lighter systems |
| Ubuntu MATE | MATE desktop | Traditional desktops and modest hardware |
| Ubuntu Studio | Creative tools | Audio, video, graphics, and photography |
| Ubuntu Unity | Unity desktop | Users who prefer the Unity workflow |
| Xubuntu | Xfce desktop | Light, stable, and configurable systems |

Flavour release schedules and support periods can differ from the main Ubuntu releases. Check the [official flavours page](https://ubuntu.com/desktop/flavours) and each flavour's website before you download.

## Release cycle and support

Ubuntu has two kinds of releases: **LTS** and **interim**.

### LTS releases

**LTS** stands for **Long Term Support**.

- A new LTS is released every two years, in April of even-numbered years.
- Each LTS gets **5 years** of free security and maintenance updates.
- LTS releases are recommended for most users, businesses, and servers.
- Ubuntu Pro can extend security coverage to **10 years**, and up to **15 years** with the Legacy add-on.

Ubuntu Pro is free for personal use on up to five machines. You do not need it to use Ubuntu normally.

LTS releases also receive **point releases**, such as 26.04.1. These are updated installers with fixes included. They are not new major versions.

### Interim releases

Interim releases come out every six months, usually in April and October.

- Each interim release gets **9 months** of support.
- They include newer kernels, drivers, and development tools.
- They are useful for testing and for very new hardware.
- They are not the best choice for production servers or long-term use.

If you use an interim release, plan to upgrade every few months or move to the next LTS.

### Current releases at a glance

The table below is accurate as of **November 2026**.

| Version | Type | Released | Standard support ends | Best for |
|---|---|---|---|---|
| Ubuntu 26.04 LTS (Resolute Raccoon) | LTS | April 2026 | May 2031 | Most users and servers |
| Ubuntu 26.10 (Stonking Stingray) | Interim | October 2026 | July 2027 | New hardware and testing |
| Ubuntu 24.04 LTS (Noble Numbat) | LTS | April 2024 | May 2029 | Users who need the previous LTS |

### Which version should you choose?

- **Most beginners:** choose the latest LTS, Ubuntu 26.04 LTS.
- **New hardware that needs a newer kernel:** try the latest interim release, but expect to upgrade sooner.
- **Servers and business systems:** choose an LTS release.
- **An older but still supported system:** Ubuntu 24.04 LTS remains supported until May 2029.

When standard support ends, a release stops receiving normal security updates. You should upgrade to a newer release before that date.

### Version numbers and codenames

Ubuntu version numbers use the year and month of release:

- **26.04** means April 2026.
- **26.10** means October 2026.

Each release also has a codename made of an adjective and an animal, such as *Resolute Raccoon* or *Noble Numbat*. Codenames are fun, but the version number is the practical detail to remember.

## Try Ubuntu before installing

You can test Ubuntu without changing your computer.

1. Download the Ubuntu Desktop ISO image.
2. [Create a bootable USB drive](../guides/bootable-usb.md).
3. Restart your computer and open the boot menu.
4. Choose the USB drive.
5. Select **Try Ubuntu** instead of installing.
6. Test your Wi-Fi, sound, display, keyboard, and touchpad.
7. Shut down and remove the USB drive when you are finished.

Nothing is installed while you use the live session. The system runs from the USB drive, so it will feel slower than an installed system. Do not use the live session for important work.

If you plan to keep Windows, read [Dual-booting Linux and Windows](../guides/dual-boot.md) before you start the installer.

## Practical examples for Linux beginners

Open the **Terminal** with `Ctrl` + `Alt` + `T` on Ubuntu Desktop. You can also search for "Terminal" in the application menu.

Commands are case-sensitive. Type a command and press `Enter` to run it. Your output may look slightly different from the examples.

### Check your Ubuntu version

```bash
cat /etc/os-release
```

You will see lines like these:

```text
PRETTY_NAME="Ubuntu 26.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="26.04"
```

This file exists on most Linux distributions, so it is a reliable way to identify the system.

### Check your hardware

```bash
nproc
free -h
df -h /
```

- `nproc` shows how many CPU threads are available.
- `free -h` shows memory in human-readable units.
- `df -h /` shows free space on the root filesystem.

These commands are safe and only display information.

### Check your kernel version

```bash
uname -r
```

The output looks similar to `7.0.0-xx-generic`. The exact number changes with kernel updates.

### Refresh the package list

```bash
sudo apt update
```

`apt` is Ubuntu's package manager for `.deb` packages. `sudo` runs the command with administrator privileges.

When you press `Enter`, Ubuntu asks for your password. Nothing appears on screen while you type. This is normal. Type your password and press `Enter`.

`apt update` refreshes the list of available packages. It does not upgrade your system by itself.

> **Safety note:** commands that start with `sudo` can change your system. Read a command carefully before running it, and never run a command you do not understand.

### Find software and read the manual

```bash
apt search gimp
```

This searches the package list for a program. It does not install anything.

```bash
man apt
```

`man` opens the manual page for a command. Use the arrow keys to scroll and press `q` to quit.

Ubuntu also uses **Snap** packages for some applications. You can explore them with:

```bash
snap find note
```

Install software from trusted sources: Ubuntu's official repositories, the Snap Store, or an official vendor website.

## Next steps

Now that you know what Ubuntu is, continue with the learning path:

1. [Download Ubuntu](./02-download.md) and choose the right image.
2. [Create a bootable USB drive](../guides/bootable-usb.md).
3. [Install Ubuntu](./03-installation.md).
4. [Complete the initial setup](./04-initial-setup.md).
5. [Set up your desktop](./05-desktop-setup.md).
6. [Troubleshoot common problems](./06-troubleshooting.md).

Useful companion guides:

- [Basic Linux commands](../guides/basic-commands.md)
- [Package managers](../guides/package-managers.md)
- [Linux desktop environments](../guides/desktop-environments.md)
- [Dual-booting Linux and Windows](../guides/dual-boot.md)

## Official resources

Official Ubuntu:

- [Ubuntu website](https://ubuntu.com/)
- [About Ubuntu](https://ubuntu.com/about)
- [Ubuntu documentation](https://documentation.ubuntu.com/)
- [Ubuntu Desktop documentation](https://ubuntu.com/desktop/docs/)
- [Ubuntu Server documentation](https://ubuntu.com/server/docs)
- [Ubuntu release cycle](https://ubuntu.com/about/release-cycle)
- [Ubuntu releases wiki](https://wiki.ubuntu.com/Releases)

Downloads:

- [Ubuntu Desktop](https://ubuntu.com/download/desktop)
- [Ubuntu Server](https://ubuntu.com/download/server)
- [All Ubuntu downloads](https://ubuntu.com/download)
- [Official Ubuntu flavours](https://ubuntu.com/desktop/flavours)

Community help:

- [Ubuntu Discourse](https://discourse.ubuntu.com/)
- [Ask Ubuntu](https://askubuntu.com/)

Related official sites:

- [Debian](https://www.debian.org/)
- [Linux kernel](https://www.kernel.org/)
- [Snapcraft](https://snapcraft.io/)
