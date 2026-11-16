# System Configuration

This section covers what to do after Linux is installed: updates, drivers,
package sources, the desktop, and the terminal.

You do not need every item on this page. Start with updates, then add the
drivers and software sources your hardware and workflow need.

## Configuration path

| Step | What you will configure | Where to go |
|---|---|---|
| 1 | First updates, drivers, codecs, firewall, and essential apps | Your distribution's **Initial Setup** page |
| 2 | Software installation and removal | [Package managers](../guides/package-managers.md) |
| 3 | Graphics drivers for Intel, AMD, or NVIDIA hardware | [Graphics drivers](../guides/graphics-drivers.md) |
| 4 | Desktop appearance, extensions, and layout | [Desktop environments](../guides/desktop-environments.md) |
| 5 | Terminal navigation and safe command use | [Basic Linux commands](../guides/basic-commands.md) |
| 6 | Regular updates, cleanup, logs, and disk checks | [System maintenance](../guides/system-maintenance.md) |

## Initial setup by distribution

- [Ubuntu initial setup](../ubuntu/04-initial-setup.md)
- [Fedora initial setup](../fedora/04-initial-setup.md)
- [Arch Linux initial setup](../arch-linux/04-initial-setup.md)
- [Debian initial setup](../debian/04-initial-setup.md)
- [Linux Mint initial setup](../linux-mint/04-initial-setup.md)
- [openSUSE initial setup](../opensuse/04-initial-setup.md)

## Common configuration tasks

### Updates

Install security and software updates from your distribution's package manager
or graphical software center. See
[Package managers](../guides/package-managers.md#updates) and
[System maintenance](../guides/system-maintenance.md#keep-the-system-updated).

### Drivers

Most Intel and AMD graphics work with the open-source drivers included in the
Linux kernel. NVIDIA systems may need a proprietary or open kernel module
provided by the distribution. See
[Graphics drivers](../guides/graphics-drivers.md).

> **Secure Boot:** Some drivers require enrolling a Machine Owner Key (MOK).
> Follow your distribution's official instructions. Do not disable Secure Boot
> as a first step.

### Package sources

Use your distribution's official repositories first. Flatpak and Snap add
sandboxed applications from separate sources. See
[Flatpak and Snap](../guides/package-managers.md#flatpak-and-snap).

### Desktop and terminal

GNOME is the default on Ubuntu and Fedora Workstation, while Linux Mint uses
Cinnamon, and openSUSE offers several desktop choices. You can install another
desktop, but mixing desktop environments can add duplicate applications and
configuration packages. See
[Desktop environments](../guides/desktop-environments.md).

For terminal basics, see [Basic Linux commands](../guides/basic-commands.md).

## Safety notes

- Keep a backup before changing drivers, disks, or boot settings.
- Read commands before running them, especially commands that start with
  `sudo`.
- Do not add third-party repositories unless you trust the publisher and know
  how to remove the repository later.
- Do not disable Secure Boot, SELinux, or the firewall as a first troubleshooting
  step.

## References

- [Ubuntu documentation](https://documentation.ubuntu.com/)
- [Fedora documentation](https://docs.fedoraproject.org/)
- [ArchWiki](https://wiki.archlinux.org/)
- [Debian documentation](https://www.debian.org/doc/)
- [Linux Mint documentation](https://linuxmint.com/documentation.php)
- [openSUSE documentation](https://doc.opensuse.org/)

[Back to documentation home](../README.md) · [Back to project README](../../README.md)
