# Linux Setup Tutorials

**Learn Linux installation and setup, step by step.**

Linux Setup Tutorials is an open-source documentation project for beginners. It
explains how to install Linux, configure a useful desktop, and prepare a system
for gaming, development, work, school, or everyday personal use.

The project focuses on practical setup tasks. It does not try to teach all of
Linux theory.

**[Browse the full documentation](docs/README.md)** · **[View the roadmap](ROADMAP.md)**

## Getting Started

New to Linux? Follow this path:

1. **Choose a distribution** from the [Linux installation](#linux-installation)
   section below.
2. **Create installation media.** Follow
   [Create a bootable USB drive](docs/guides/bootable-usb.md).
3. **Install Linux.** Open your distribution's learning path and follow the
   download, installation, and initial setup pages.
4. **Configure your system.** Add drivers, package sources, updates, and desktop
   tools from [System configuration](docs/system-configuration/README.md).
5. **Prepare your use case.** Choose a setup from
   [Setups by use case](#setups-by-use-case).
6. **Learn to maintain it.** Keep backups, updates, and recovery steps ready in
   [Maintenance](docs/maintenance/README.md).

> Always back up important files before installing Linux or changing disk
> partitions. Installing an operating system can erase a disk if you choose the
> wrong option.

## Linux Installation

Choose a distribution and follow its six-page learning path: introduction,
download, installation, initial setup, desktop setup, and troubleshooting.

| Distribution | Description | Start here | Official website |
|---|---|---|---|
| Ubuntu | Beginner-friendly Linux desktop | [Ubuntu guide](docs/ubuntu/README.md) | [ubuntu.com](https://ubuntu.com/) |
| Fedora | Modern Linux for desktop users and developers | [Fedora guide](docs/fedora/README.md) | [fedoraproject.org](https://fedoraproject.org/) |
| Arch Linux | Flexible and customizable Linux | [Arch Linux guide](docs/arch-linux/README.md) | [archlinux.org](https://archlinux.org/) |
| Debian | Stable and community-driven Linux | [Debian guide](docs/debian/README.md) | [debian.org](https://www.debian.org/) |
| Linux Mint | Easy-to-use desktop Linux | [Linux Mint guide](docs/linux-mint/README.md) | [linuxmint.com](https://linuxmint.com/) |
| openSUSE | Linux for desktops and developers | [openSUSE guide](docs/opensuse/README.md) | [opensuse.org](https://www.opensuse.org/) |

Related installation guides:

- [Create a bootable USB drive](docs/guides/bootable-usb.md)
- [Dual-booting Linux and Windows](docs/guides/dual-boot.md)

## System Configuration

After installation, configure the parts of the system you use every day.

| Topic | What it covers | Guide |
|---|---|---|
| Initial setup | Updates, drivers, essential software, and first settings | [System configuration hub](docs/system-configuration/README.md) |
| Package managers | Installing and removing software with apt, dnf, pacman, zypper, Flatpak, and Snap | [Package managers](docs/guides/package-managers.md) |
| Graphics drivers | Intel, AMD, and NVIDIA driver setup | [Graphics drivers](docs/guides/graphics-drivers.md) |
| Desktop environments | GNOME, KDE Plasma, Xfce, Cinnamon, and MATE | [Desktop environments](docs/guides/desktop-environments.md) |
| Terminal basics | Navigating the shell and running commands safely | [Basic Linux commands](docs/guides/basic-commands.md) |
| Updates and cleanup | Keeping the system updated and removing unused files | [System maintenance](docs/guides/system-maintenance.md) |

## Setups by Use Case

Choose the setup that matches how you use your computer. You can follow more
than one.

| Setup | Best for | Start here |
|---|---|---|
| Gaming | Playing games on Linux | [Gaming setup](docs/categories/gaming/README.md) |
| Development | Writing code and building software | [Development setup](docs/categories/development/README.md) |
| Work | Office tasks, email, and video calls | [Work setup](docs/categories/work/README.md) |
| School | Studying, notes, and online classes | [School setup](docs/categories/school/README.md) |
| Personal | Everyday home use | [Personal setup](docs/categories/personal/README.md) |

See [all setup categories](docs/categories/README.md) for a short comparison and
common next steps.

## Troubleshooting

Start with the [troubleshooting hub](docs/troubleshooting/README.md) for safe
first checks. Each distribution also has a troubleshooting page:

- [Ubuntu troubleshooting](docs/ubuntu/06-troubleshooting.md)
- [Fedora troubleshooting](docs/fedora/06-troubleshooting.md)
- [Arch Linux troubleshooting](docs/arch-linux/06-troubleshooting.md)
- [Debian troubleshooting](docs/debian/06-troubleshooting.md)
- [Linux Mint troubleshooting](docs/linux-mint/06-troubleshooting.md)
- [openSUSE troubleshooting](docs/opensuse/06-troubleshooting.md)

## Maintenance

- [Backup and restore](docs/maintenance/backup-and-restore.md)
- [System maintenance guide](docs/guides/system-maintenance.md)
- [Maintenance hub](docs/maintenance/README.md)

## Contributing

Contributions are welcome. You can improve tutorials, fix errors, report broken
links, or translate guides.

- Open an issue for large changes so we can agree on the approach.
- Keep each page focused on one topic and test every command you document.
- Write for someone installing Linux for the first time. Use short sentences,
  numbered steps, and explain why a step matters.
- Warn before destructive commands and offer a safe rollback.
- Follow the [setup tutorial template](docs/templates/setup-tutorial.md). Do
  not add automatic installation scripts unless the project agrees to maintain
  and test them.

By contributing, you agree to license your work under the project license for
that content type. See [LICENSES/README.md](LICENSES/README.md) for licensing
details.

## Official Documentation

- [Ubuntu Documentation](https://documentation.ubuntu.com/)
- [Fedora Documentation](https://docs.fedoraproject.org/)
- [ArchWiki](https://wiki.archlinux.org/)
- [Debian Documentation](https://www.debian.org/doc/)
- [Linux Mint Documentation](https://linuxmint.com/documentation.php)
- [openSUSE Documentation](https://doc.opensuse.org/)

## License

This project is available under open-content and open-source licenses.

- Documentation is licensed under [CC BY-SA 4.0](LICENSE).
- Scripts and configuration files are licensed under the
  [MIT License](LICENSES/MIT).

See [COPYING](LICENSES/COPYING) and [LICENSES](LICENSES/README.md) for
licensing details.
