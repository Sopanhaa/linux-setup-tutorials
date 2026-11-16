# Initial Setup — Fedora

**Supported version:** Fedora Workstation 45

**Difficulty:** Beginner

**Terminal required:** Yes for updates, Flatpak, drivers, and security checks.

**Last reviewed:** Not yet reviewed on live systems.

**Learning path:** ← [Installation](./03-installation.md) · [Fedora overview](./README.md) · [Desktop Setup](./05-desktop-setup.md) →

## Overview

This tutorial turns a fresh Fedora Workstation installation into a usable daily
system. You will install updates, check DNF and Flatpak, add optional software
sources, check drivers, and verify the firewall and SELinux.

The commands use **DNF5**, the package manager in Fedora 45. Run them in a
terminal opened from the application menu.

## Requirements

- Fedora Workstation 45 installed and able to boot.
- An internet connection.
- Your user account must be able to run `sudo`. The first user created during
  GNOME Initial Setup is normally in the `wheel` group.
- At least 10 GB of free disk space for updates and applications.
- A backup plan for your personal files.

> **Before you change drivers or firmware:** connect the computer to power. A
> power loss during a firmware update can make the device unusable.

## Installation

### Step 1: Finish GNOME Initial Setup

If you have not completed the first-boot wizard, do that first:

1. Sign in with your user account.
2. Connect to Wi-Fi or Ethernet.
3. Set the time zone.
4. Review the privacy settings.
5. Decide whether to enable third-party repositories. You can change this later
   in **Software → Software Repositories**.

Third-party repositories include sources such as Flathub and vendor
repositories. They can contain proprietary software. Enable them only if you
need them.

### Step 2: Update Fedora

Refresh the repository metadata and install updates:

```bash
sudo dnf upgrade --refresh
```

- `sudo` runs the command as an administrator.
- `dnf` is Fedora's package manager. Fedora 45 uses DNF5.
- `upgrade` installs newer versions of installed packages.
- `--refresh` downloads current repository metadata before checking for updates.

Read the transaction summary before confirming. If a kernel or driver update is
included, restart after the upgrade finishes:

```bash
sudo reboot
```

Check the DNF version and enabled repositories:

```bash
dnf --version
sudo dnf repolist
```

`dnf --version` shows the DNF implementation and version. `dnf repolist` lists
the repositories DNF will use.

### Step 3: Remove unused dependencies (optional)

After updates, you can remove packages that were installed as dependencies and
are no longer required:

```bash
sudo dnf autoremove
```

Read the list before confirming. `autoremove` can remove packages you still need
if they were installed only as dependencies. If you are unsure, skip this step.

### Step 4: Check Flatpak and Flathub

Fedora Workstation includes Flatpak. Check it:

```bash
flatpak --version
flatpak remotes
```

`flatpak remotes` lists the application sources, such as Fedora Flatpaks and
Flathub.

If Flathub is missing, add it:

```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

- `remote-add` adds a Flatpak repository.
- `--if-not-exists` prevents an error if the remote is already configured.
- `flathub` is the remote name used by later commands.

Install a test application from Flathub:

```bash
flatpak install flathub org.gimp.GIMP
```

Run it from the application menu or with:

```bash
flatpak run org.gimp.GIMP
```

Update or remove it:

```bash
flatpak update
flatpak uninstall org.gimp.GIMP
```

Flatpak applications are sandboxed. They may not have access to all files or
devices by default. Use the application's settings or a permission tool to grant
only the access it needs.

> **Fedora Flatpaks and Flathub:** Fedora Workstation can show both sources.
> They may provide different builds of the same application. Pick one source per
> application to avoid duplicate runtimes and confusing updates.

### Step 5: Enable RPM Fusion and multimedia codecs (optional)

Fedora's official repositories do not include some multimedia codecs because of
patent and licensing rules. RPM Fusion provides additional packages.

This step is optional. If you only use free formats, you do not need it.

> **Third-party warning:** RPM Fusion is a community repository, not an official
> Fedora repository. Review its documentation and enable only the repositories
> you need.

Enable RPM Fusion Free and Nonfree:

```bash
sudo dnf install \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
  https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

- `$(rpm -E %fedora)` expands to your Fedora release number, such as `45`.
- The two packages add the Free and Nonfree RPM Fusion repositories.
- Read the GPG key prompt before accepting.

Verify the repositories:

```bash
dnf repolist | grep -i rpmfusion
```

Install common multimedia support from Fedora first:

```bash
sudo dnf group install multimedia
```

For extended codecs, follow the official RPM Fusion multimedia guide. One common
step is replacing Fedora's limited `ffmpeg-free` package with the full build:

```bash
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
```

> **Warning:** `--allowerasing` allows DNF to remove conflicting packages. Read
> the transaction list carefully. Do not continue if it proposes removing an
> application or package you need.

Then install the RPM Fusion multimedia group:

```bash
sudo dnf install @multimedia \
  --setopt="install_weak_deps=False" \
  --exclude=PackageKit-gstreamer-plugin
```

Install Cisco's OpenH264 plugin for Firefox video calls:

```bash
sudo dnf install gstreamer1-plugin-openh264 mozilla-openh264
```

OpenH264 is provided by Cisco through a Fedora repository. It enables H.264 in
WebRTC contexts, not all H.264 video playback.

### Step 6: Check graphics drivers

Identify your graphics hardware:

```bash
lspci -k | grep -A 3 -E 'VGA|3D'
```

- `lspci` lists PCI devices.
- `-k` shows the kernel driver in use.
- `grep -A 3` prints the matching line and the next three lines.

Intel and AMD graphics normally use the open-source kernel drivers and Mesa.
Fedora includes them. Install a tool to check OpenGL:

```bash
sudo dnf install glx-utils
glxinfo -B
```

The renderer should name your GPU, not `llvmpipe`. `llvmpipe` means software
rendering and usually indicates a driver problem.

NVIDIA graphics need a separate driver. Do not install random NVIDIA packages
from the internet. Follow [Graphics drivers](../guides/graphics-drivers.md) and
the official RPM Fusion guide. Secure Boot may require enrolling a Machine Owner
Key (MOK).

### Step 7: Verify the firewall and SELinux

Fedora Workstation enables **firewalld** and **SELinux** by default. Check them
instead of disabling them:

```bash
getenforce
sudo firewall-cmd --state
sudo firewall-cmd --get-default-zone
sudo firewall-cmd --list-all
```

Expected results:

- `getenforce` prints `Enforcing`.
- `firewall-cmd --state` prints `running`.
- The default zone is usually `FedoraWorkstation` on Workstation.
- `firewall-cmd --list-all` shows the allowed services and ports.

> **Workstation firewall note:** The `FedoraWorkstation` zone allows a wide range
> of high ports by default to support desktop applications. Do not assume the
> firewall is a complete security boundary. If you use the computer on untrusted
> networks, read the firewalld documentation before changing zones.

Do not disable SELinux or firewalld as a first troubleshooting step. Fix the
specific denial or blocked service instead.

### Step 8: Check firmware updates (optional)

Fedora can update firmware through `fwupd`:

```bash
sudo fwupdmgr refresh
sudo fwupdmgr get-updates
```

- `refresh` downloads current firmware metadata.
- `get-updates` lists available updates without installing them.

If updates are listed, read the details and connect the computer to power before
running:

```bash
sudo fwupdmgr update
```

Do not unplug power or force a shutdown during a firmware update. Some updates
restart the computer automatically.

### Step 9: Set up backups

Install a graphical backup tool if you want one:

```bash
sudo dnf install deja-dup
```

Déjà Dup backs up personal files. It does not back up the whole operating system.
For a complete backup plan, including restore testing, see
[Backup and restore](../maintenance/backup-and-restore.md).

## Configuration

### Check the time zone and hostname

```bash
timedatectl
hostnamectl
```

If the time zone is wrong, set it in **Settings → Date & Time** or with:

```bash
sudo timedatectl set-timezone Asia/Phnom_Penh
```

Replace `Asia/Phnom_Penh` with your zone. List zones with:

```bash
timedatectl list-timezones
```

### Configure automatic updates

Fedora Workstation shows update notifications through GNOME Software. You can
also install updates manually with `sudo dnf upgrade --refresh`.

If you want automatic DNF updates, read the official `dnf-automatic`
documentation before enabling a timer. Automatic updates can restart services or
require a reboot at inconvenient times.

### Choose software sources

Use **Software → Software Repositories** to review enabled sources. Keep Fedora's
official repositories enabled. Enable third-party sources only when you need
them, and know how to remove them later.

### Configure Flatpak permissions

Flatpak applications run in a sandbox. If an application cannot open a file or
use a device, grant the smallest permission it needs. Do not disable the sandbox
for every application.

## Verification

Run these checks after the setup:

```bash
cat /etc/os-release
dnf --version
sudo dnf repolist
flatpak remotes
getenforce
sudo firewall-cmd --state
```

Expected results:

- `/etc/os-release` shows Fedora Linux 45.
- `dnf --version` shows DNF5.
- `dnf repolist` shows Fedora repositories, plus RPM Fusion if you enabled it.
- `flatpak remotes` shows Fedora Flatpaks and/or Flathub.
- `getenforce` shows `Enforcing`.
- `firewall-cmd --state` shows `running`.

Check the graphics driver:

```bash
lspci -k | grep -A 3 -E 'VGA|3D'
glxinfo -B
```

Restart the computer and confirm that:

- [ ] The desktop starts normally.
- [ ] Networking works.
- [ ] Sound works.
- [ ] The firewall is still running.
- [ ] SELinux is still enforcing.
- [ ] Your backup tool can see the backup drive.

## Troubleshooting

### `Could not get lock` or a DNF transaction is already running

Another package operation is running. Wait for it to finish, or restart the
computer. Do not delete DNF lock files.

### `dnf upgrade` reports a repository error

- Read the repository name in the error.
- Check the internet connection.
- Confirm that the repository supports Fedora 45.
- Disable a third-party repository that is unavailable or no longer maintained.
- Do not mix packages from different Fedora releases.

### Fedora 45 reports a package signature error

Fedora 45 enforces RPM signature checking by default. If a package reports
`NOKEY` or a signature failure:

- Confirm that the repository's GPG key is installed correctly.
- Reinstall the repository release package from its official source.
- Do not use `--nogpgcheck` or `--nosignature` to bypass the error.

### I cannot find Fedora's repository files in `/etc/yum.repos.d/`

Fedora 45 moved its own repository configuration to
`/usr/share/dnf5/repos.d/`. Third-party repositories usually remain in
`/etc/yum.repos.d/`. Use `dnf repolist` to see what is active instead of relying
on the folder.

### Flatpak cannot find an application

- Check the remotes:
  ```bash
  flatpak remotes
  ```
- Update the application metadata:
  ```bash
  flatpak update --appstream
  ```
- Search with the application ID or name:
  ```bash
  flatpak search gimp
  ```

### RPM Fusion packages conflict with Fedora packages

Read the DNF output before confirming. `--allowerasing` may remove packages.
Remove the conflicting third-party package or follow RPM Fusion's current
instructions. Do not force the transaction with unsafe options.

### NVIDIA driver causes a black screen

- Boot an older kernel from the GRUB menu or use recovery mode.
- Remove the NVIDIA driver and return to the open-source driver.
- Read [Graphics drivers](../guides/graphics-drivers.md#troubleshooting).

### SELinux denies an application

- Do not set SELinux to permissive as a first step.
- Read the denial:
  ```bash
  sudo ausearch -m AVC -ts recent
  ```
- Follow the official SELinux troubleshooting guide for the specific denial.
- Use `sealert` if the `setroubleshoot-server` package is installed.

### The firewall blocks a local service

Check whether the service is listening before blaming the firewall:

```bash
ss -tlnp
```

Then inspect the active zone:

```bash
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --list-all
```

Open only the port or service you need, and remove the rule when it is no longer
required.

## Uninstall or Restore

### Remove optional packages

Remove packages you added and no longer need:

```bash
sudo dnf remove package-name
```

Read the transaction list. Removing a package can also remove applications that
depend on it.

### Remove Flatpak applications

```bash
flatpak uninstall org.gimp.GIMP
flatpak uninstall --unused
```

The first command removes the application. The second removes runtimes that are
no longer used.

### Remove RPM Fusion

Remove the RPM Fusion release packages:

```bash
sudo dnf remove rpmfusion-free-release rpmfusion-nonfree-release
```

Then remove packages installed from RPM Fusion if you no longer need them. Some
multimedia packages may be replaced by Fedora packages; follow RPM Fusion's
documentation and review the transaction.

### Disable third-party repositories

Open **Software → Software Repositories** and turn off the sources you no longer
want. You can also use:

```bash
sudo fedora-third-party disable
```

This disables all third-party repositories managed by Fedora, including Flathub.
Review the result with:

```bash
dnf repolist
flatpak remotes --show-disabled
```

### Restore personal files

Use Déjà Dup or `rsync` to restore from your backup. See
[Backup and restore](../maintenance/backup-and-restore.md#uninstall-or-restore).

## References

- [Fedora 45 release notes](https://docs.fedoraproject.org/en-US/release-notes/f45/)
- [Fedora Workstation documentation](https://docs.fedoraproject.org/en-US/workstation-docs/)
- [Fedora Quick Docs: DNF](https://docs.fedoraproject.org/en-US/quick-docs/dnf/)
- [Fedora Quick Docs: Flatpak and Flathub](https://docs.fedoraproject.org/en-US/quick-docs/flathub-flatpak-applications/)
- [Fedora Workstation third-party repositories](https://docs.fedoraproject.org/en-US/workstation-working-group/third-party-repos/)
- [Installing plugins for playing movies and music](https://docs.fedoraproject.org/en-US/quick-docs/installing-plugins-for-playing-movies-and-music)
- [RPM Fusion configuration](https://rpmfusion.org/Configuration)
- [RPM Fusion multimedia guide](https://rpmfusion.org/Howto/Multimedia)
- [Fedora SELinux documentation](https://docs.fedoraproject.org/en-US/quick-docs/selinux-getting-started/)
- [firewalld documentation](https://firewalld.org/documentation/)
- [Configuring IP networking with nmcli](https://docs.fedoraproject.org/en-US/quick-docs/configuring-ip-networking-with-nmcli/)
- [Automatic updates with DNF](https://docs.fedoraproject.org/en-US/quick-docs/autoupdates/)
- [Backup and restore](../maintenance/backup-and-restore.md)

**Next:** [Set up your desktop](./05-desktop-setup.md)
