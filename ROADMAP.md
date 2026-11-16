# Roadmap

This roadmap tracks practical Linux setup tutorials. A task is only marked
**Done** after its tutorial has been reviewed and tested on the stated
distribution and version.

## Status definitions

| Status | Meaning |
|---|---|
| **Todo** | Planned, but no tutorial content exists yet. |
| **In Progress** | Content is being written or is incomplete. |
| **In Review** | A draft exists and needs technical review, link checks, and testing. |
| **Done** | Reviewed and verified. The page states which versions were tested. |

## P0 — Essential

| Task | Status | Documentation | Notes |
|---|---|---|---|
| Ubuntu installation and first-time setup | In Review | [Ubuntu guide](docs/ubuntu/README.md) | Download, installation, initial setup, desktop setup, and troubleshooting pages drafted. Needs testing on Ubuntu 26.04 LTS. |
| Bootable USB with Rufus and Ventoy | In Review | [Create a bootable USB drive](docs/guides/bootable-usb.md) | Existing beginner tutorial, restructured to the standard format. Needs a fresh link check and boot test before marking Done. |
| Basic Linux initial configuration | In Review | [System configuration](docs/system-configuration/README.md), [Ubuntu initial setup](docs/ubuntu/04-initial-setup.md) | Covers updates, drivers, codecs, firewall, Flatpak, and backups. Needs distribution-by-distribution testing. |

## P1 — Important

| Task | Status | Documentation | Notes |
|---|---|---|---|
| Fedora installation and setup | Todo | [Fedora guide](docs/fedora/README.md) | All six pages are currently stubs. |
| NVIDIA graphics driver setup | In Review | [Graphics drivers](docs/guides/graphics-drivers.md) | Cross-distribution guide drafted. Needs testing with Secure Boot and at least one NVIDIA GPU. |
| Flatpak and Snap installation and configuration | In Review | [Package managers](docs/guides/package-managers.md#flatpak-and-snap) | Commands and verification steps drafted. Needs desktop integration testing. |
| Git, VS Code, and .NET development setup | Todo | [Development setup](docs/categories/development/README.md) | Category overview exists. A step-by-step tutorial is still needed. |

## P2 — Future

| Task | Status | Documentation | Notes |
|---|---|---|---|
| GNOME customization | Todo | [Ubuntu desktop setup](docs/ubuntu/05-desktop-setup.md) | Basic desktop configuration exists; a deeper theme and extension tutorial is planned. |
| Linux gaming with Steam and Proton | Todo | [Gaming setup](docs/categories/gaming/README.md) | Category overview exists. Needs a tested Steam and Proton walkthrough. |
| Windows and Linux dual boot | Todo | [Dual-booting Linux and Windows](docs/guides/dual-boot.md) | Stub page. Needs UEFI, Secure Boot, and BitLocker guidance. |
| Backup and recovery setup | In Review | [Backup and restore](docs/maintenance/backup-and-restore.md) | Tutorial drafted with file backups, Timeshift, verification, and restore steps. Needs testing on all listed distributions. |

## Other known gaps

- Fedora, Arch Linux, Debian, Linux Mint, and openSUSE still have stub pages
  outside Ubuntu.
- General guides for basic commands, desktop environments, dual boot, and system
  maintenance are still stubs or need review.
- The project license is not selected yet.

## How to update this roadmap

1. Change a task to **In Progress** when someone starts writing.
2. Change it to **In Review** when the draft is complete and links resolve.
3. Change it to **Done** only after testing and review. Add the tested
   distribution and version to the tutorial.
4. Add new tasks with a priority, documentation path, and one-sentence note.

[Back to project README](README.md) · [Documentation home](docs/README.md)
