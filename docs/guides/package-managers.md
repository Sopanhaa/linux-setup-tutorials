# Package Managers

**Supported systems:** Ubuntu, Debian, Linux Mint, Fedora, Arch Linux, and
openSUSE

**Difficulty:** Beginner

**Terminal required:** Yes

**Last reviewed:** Not yet reviewed on live systems.

**General guides:** [All guides](./README.md)

## Overview

A package manager installs, updates, and removes software from trusted software
repositories. Each Linux distribution has its own package manager:

| Distribution family | Package manager | Package format |
|---|---|---|
| Ubuntu, Debian, Linux Mint | `apt` | `.deb` |
| Fedora | `dnf` | `.rpm` |
| Arch Linux | `pacman` | `.pkg.tar.zst` |
| openSUSE | `zypper` | `.rpm` |
| Cross-distribution desktop apps | Flatpak | Flatpak |
| Cross-distribution apps, Ubuntu default | Snap | Snap |

This tutorial shows the common commands. Use your distribution's section. Do
not mix package managers for the same system package.

> **Safety:** Package managers change system files. Read the list of packages
> before confirming an installation or removal. Commands with `sudo` run as an
> administrator.

## Requirements

- A supported Linux system with internet access.
- A user account that can run `sudo`.
- Enough free disk space for downloads and installed packages.
- A backup of important files before large upgrades.

## Installation

### Ubuntu, Debian, and Linux Mint — apt

Refresh the package list:

```bash
sudo apt update
```

Search for software:

```bash
apt search gimp
```

Show details before installing:

```bash
apt show gimp
```

Install a package:

```bash
sudo apt install gimp
```

Remove a package but keep system-wide configuration:

```bash
sudo apt remove gimp
```

Remove configuration too:

```bash
sudo apt purge gimp
```

Remove packages that are no longer needed:

```bash
sudo apt autoremove
```

Upgrade installed packages:

```bash
sudo apt upgrade
```

`apt update` refreshes package lists. `apt upgrade` installs newer versions of
installed packages. They are different commands.

### Fedora — dnf

Refresh metadata and check for updates:

```bash
sudo dnf check-update
```

Search and show details:

```bash
dnf search gimp
dnf info gimp
```

Install and remove:

```bash
sudo dnf install gimp
sudo dnf remove gimp
```

Upgrade installed packages:

```bash
sudo dnf upgrade --refresh
```

Remove unused dependencies:

```bash
sudo dnf autoremove
```

### Arch Linux — pacman

Update the package database and upgrade the system:

```bash
sudo pacman -Syu
```

Search for a package:

```bash
pacman -Ss gimp
```

Install a package:

```bash
sudo pacman -S gimp
```

Remove a package and its unused dependencies:

```bash
sudo pacman -Rns gimp
```

List installed packages:

```bash
pacman -Q
```

> **Arch Linux warning:** Do not perform partial upgrades. Update the whole
> system with `sudo pacman -Syu` before installing new packages. Mixing old and
> new packages can break the system. Read the
> [ArchWiki pacman page](https://wiki.archlinux.org/title/Pacman) before changing
> repositories.

### openSUSE — zypper

Refresh repositories:

```bash
sudo zypper refresh
```

Search and show details:

```bash
zypper search gimp
zypper info gimp
```

Install and remove:

```bash
sudo zypper install gimp
sudo zypper remove gimp
```

Update installed packages:

```bash
sudo zypper update
```

On openSUSE Tumbleweed, use the distribution upgrade command instead:

```bash
sudo zypper dup
```

`zypper update` updates packages within the current release. `zypper dup` moves
the system to the latest Tumbleweed snapshot. Use the command recommended for
your openSUSE edition.

### Flatpak and Snap

Flatpak and Snap both install applications in sandboxed environments. They can
be used alongside your distribution's package manager.

#### Flatpak

Flatpak runs desktop applications in a sandbox and works across distributions.

Install Flatpak:

**Ubuntu, Debian, Linux Mint:**

```bash
sudo apt install flatpak
```

**Fedora:**

```bash
sudo dnf install flatpak
```

**Arch Linux:**

```bash
sudo pacman -S flatpak
```

**openSUSE:**

```bash
sudo zypper install flatpak
```

Add Flathub, the main Flatpak application repository:

```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

Install an application by its application ID:

```bash
flatpak install flathub org.gimp.GIMP
```

Run it:

```bash
flatpak run org.gimp.GIMP
```

Update or remove it:

```bash
flatpak update
flatpak uninstall org.gimp.GIMP
```

Remove unused runtimes:

```bash
flatpak uninstall --unused
```

To browse applications with a graphical tool, install your distribution's
Flatpak software-center plugin, such as `gnome-software-plugin-flatpak` on
Ubuntu. Sign out and back in after installing the plugin.

#### Snap

Snap is the default package format for some Ubuntu applications.

On Ubuntu, check installed snaps:

```bash
snap list
```

Search and install a test snap:

```bash
snap find hello-world
sudo snap install hello-world
```

Update or remove:

```bash
sudo snap refresh
sudo snap remove hello-world
```

On other distributions, install `snapd` from your package manager and follow the
official Snap installation instructions for your distribution. Do not copy
commands from an unrelated distribution without checking them.

## Configuration

### Updates

Update commands are listed in each package manager section above. Run the update
command for your distribution before installing new software, especially after a
long time without updates. Read the output before confirming.

### Keep official repositories first

Use your distribution's official repositories unless you need a specific
third-party package. Third-party repositories can become outdated, change
signing keys, or conflict with official packages.

### Add a repository carefully

Each distribution has its own tool:

- **Ubuntu/Debian:** `sudo add-apt-repository` for supported PPAs.
- **Fedora:** `sudo dnf config-manager` for official or vendor repositories.
- **Arch Linux:** edit `/etc/pacman.conf` only after reading the ArchWiki.
- **openSUSE:** `sudo zypper addrepo` or YaST.

After adding a repository, refresh the package list and install only the package
you need. Record how to remove the repository later.

> **Trust warning:** A repository can install software that runs with your user
> permissions and, for system packages, with administrator permissions. Add only
> repositories published by the software vendor or your distribution.

### Choose a Snap channel

Snap packages can have stable, candidate, beta, and edge channels. Use the stable
channel for daily work:

```bash
sudo snap install notes --channel=latest/stable
```

Do not use `edge` unless you are testing and can recover from breakage.

### Clean the package cache

Package managers keep downloaded packages in a cache. To free space:

```bash
sudo apt clean
```

```bash
sudo dnf clean all
```

```bash
sudo pacman -Sc
```

```bash
sudo zypper clean
```

Read the prompt before confirming. These commands remove cached package files,
not installed programs.

## Verification

Check that the package manager is available:

```bash
apt --version
dnf --version
pacman --version
zypper --version
flatpak --version
snap version
```

You only need the command for your distribution. A missing command is normal if
that package manager is not installed.

Check Flatpak remotes:

```bash
flatpak remotes
```

Check Snap packages:

```bash
snap list
```

Check that an installed application starts from the application menu. For a
Flatpak application, you can also run it with `flatpak run <application-id>`.

## Troubleshooting

### `Could not get lock` on Ubuntu or Debian

Another package operation is running. Wait for it to finish, or restart the
computer. Do not delete the lock file.

### A repository returns `404 Not Found`

- The repository may not support your distribution version.
- The repository may have moved or been discontinued.
- Disable or remove the repository, then refresh the package list.

### A GPG key error appears

The repository's signing key is missing or has changed. Follow the repository
vendor's official instructions to install the current key. Do not disable
signature checking to bypass the error.

### `No space left on device`

Check free space:

```bash
df -h
```

Clean the package cache and remove unused packages. See
[System maintenance](./system-maintenance.md#remove-unused-software).

### Arch Linux reports a partial upgrade

Run a full system upgrade:

```bash
sudo pacman -Syu
```

Do not install a package with `-S` alone if the system has not been updated. If
the upgrade fails, read the ArchWiki and the news feed before forcing anything.

### Snap commands fail on a non-Ubuntu system

- Confirm that `snapd` is installed and running.
- Check the official Snap documentation for your distribution.
- Do not create symbolic links or enable services unless the official
  instructions tell you to.

### Flatpak cannot find an application

- Confirm that Flathub is added:
  ```bash
  flatpak remotes
  ```
- Search with the application ID or name:
  ```bash
  flatpak search gimp
  ```
- Update the remote metadata:
  ```bash
  flatpak update --appstream
  ```

## Uninstall or Restore

### Remove a package

Use the remove command for your distribution:

```bash
sudo apt remove package-name
sudo dnf remove package-name
sudo pacman -Rns package-name
sudo zypper remove package-name
```

### Remove a Flatpak application

```bash
flatpak uninstall org.gimp.GIMP
flatpak uninstall --unused
```

### Remove a Snap application

```bash
sudo snap remove hello-world
```

### Remove a third-party repository

Use the same tool that added it:

- Ubuntu/Debian: `sudo add-apt-repository --remove <repository>`
- Fedora: `sudo dnf config-manager --set-disabled <repository-id>` on releases
  that provide this option. On DNF5, use
  `sudo dnf config-manager setopt <repository-id>.enabled=0`.
- openSUSE: `sudo zypper removerepo <repository-name>`
- Arch Linux: remove the repository entry from `/etc/pacman.conf`

Refresh the package list after removing a repository. If a package was installed
from that repository, remove or replace it before disabling the source.

### Restore a broken package state

For Ubuntu and Debian:

```bash
sudo dpkg --configure -a
sudo apt --fix-broken install
```

Read the output before confirming. For other distributions, use the package
manager's official repair or transaction-history tools.

## References

- [Debian APT documentation](https://www.debian.org/doc/manuals/apt-guide/)
- [Ubuntu package management](https://ubuntu.com/server/docs/package-management)
- [Fedora DNF documentation](https://docs.fedoraproject.org/en-US/quick-docs/dnf/)
- [ArchWiki: pacman](https://wiki.archlinux.org/title/Pacman)
- [openSUSE package management](https://doc.opensuse.org/)
- [Flatpak documentation](https://docs.flatpak.org/)
- [Flathub](https://flathub.org/)
- [Snap documentation](https://snapcraft.io/docs)
