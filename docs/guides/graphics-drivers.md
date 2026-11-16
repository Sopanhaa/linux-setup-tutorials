# Graphics Drivers

**Supported systems:** Ubuntu, Debian, Linux Mint, Fedora, Arch Linux, and
openSUSE

**Difficulty:** Intermediate

**Terminal required:** Yes

**Last reviewed:** Not yet reviewed on live systems.

**General guides:** [All guides](./README.md)

## Overview

Linux graphics drivers fall into two groups:

- **Open-source drivers** are included with the kernel and Mesa. They are the
  default for Intel and AMD graphics and work well for most desktop and gaming
  use.
- **Proprietary or vendor drivers** are separate packages, usually for NVIDIA
  graphics. They can improve performance and support features that the
  open-source driver does not provide, but they can also break after a kernel
  update.

This tutorial helps you identify your GPU, install the driver recommended by
your distribution, and verify which driver is active.

> **Warning:** A broken graphics driver can prevent the desktop from starting.
> Before changing drivers, make sure you can boot an older kernel or recovery
> mode, and back up important files.

## Requirements

- A working Linux installation with internet access.
- `sudo` access.
- The computer connected to power. Do not change drivers on low battery.
- Knowledge of your GPU model.
- For Secure Boot systems, the ability to enroll a Machine Owner Key (MOK) if
  your distribution requires it.
- A backup and a recovery plan.

### Identify your GPU

```bash
lspci -k | grep -A 3 -E 'VGA|3D'
```

- `lspci` lists PCI devices.
- `-k` shows the kernel driver in use.
- `grep -A 3` prints the matching line and the next three lines.

Look for `VGA compatible controller` or `3D controller`, the vendor name, and the
`Kernel driver in use` line. Do not continue until you know whether you have
Intel, AMD, or NVIDIA graphics.

## Installation

### Intel graphics

Intel graphics use the open-source `i915` or `xe` kernel driver and Mesa. You
normally do not install a separate driver.

Keep the kernel and Mesa updated through your distribution's package manager.
To install a tool that reports OpenGL information:

```bash
sudo apt install mesa-utils
```

On Fedora:

```bash
sudo dnf install glx-utils
```

On Arch Linux:

```bash
sudo pacman -S mesa-utils
```

On openSUSE, search for the package that provides `glxinfo`:

```bash
zypper search --provides glxinfo
```

Install the matching package with `sudo zypper install <package-name>`.

### AMD graphics

AMD graphics use the open-source `amdgpu` kernel driver and Mesa. Most systems
need no separate driver installation.

Keep the kernel and Mesa updated. For newer AMD GPUs, a newer kernel may be
required.

> **Professional workloads:** AMD occasionally provides a separate proprietary
> stack for professional applications. Do not install it for normal desktop or
> gaming use unless the application vendor requires it.

### NVIDIA graphics

NVIDIA drivers are distributed separately. The correct package name and version
depend on your distribution and kernel.

> **Secure Boot:** Many distributions require you to enroll a key before a
> third-party kernel module can load. Follow your distribution's official
> instructions. Do not disable Secure Boot as a first step.

#### Ubuntu and Linux Mint

List devices and recommended drivers:

```bash
ubuntu-drivers devices
```

Install the recommended driver:

```bash
sudo ubuntu-drivers install
```

You can also open **Software & Updates → Additional Drivers**, select a driver,
and click **Apply Changes**.

If Secure Boot is enabled, Ubuntu may ask you to create and enroll a MOK password
during installation. Reboot when prompted and follow the on-screen enrollment
steps. If you miss the prompt, the driver may not load until you enroll the key.

Official reference:
[Ubuntu NVIDIA driver installation](https://ubuntu.com/server/docs/nvidia-drivers-installation)

#### Fedora

Fedora does not include the NVIDIA proprietary driver in its default
repositories. Enable RPM Fusion first, then install the driver:

```bash
sudo dnf install \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
  https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

```bash
sudo dnf install akmod-nvidia
```

The `akmod` package builds the kernel module for your running kernel. Wait a
minute or two for the build to finish before rebooting. You can check whether the
module is available with:

```bash
modinfo nvidia
```

If Secure Boot is enabled, enroll the RPM Fusion signing key:

```bash
sudo kmodgenca -a
sudo mokutil --import /etc/pki/akmods/certs/public_key.der
```

Reboot and complete the MOK enrollment screen. The exact steps are documented by
RPM Fusion; read them before running the commands.

Official reference:
[RPM Fusion NVIDIA how-to](https://rpmfusion.org/Howto/NVIDIA)

#### Arch Linux

Install the driver that matches your kernel:

```bash
sudo pacman -S nvidia nvidia-utils
```

- Use `nvidia-lts` instead of `nvidia` if you use the `linux-lts` kernel.
- Use `nvidia-dkms` if you use a custom kernel.

Read the [ArchWiki NVIDIA page](https://wiki.archlinux.org/title/NVIDIA) before
installing. It explains kernel parameters, Wayland, PRIME, and laptop setups.

#### openSUSE

Use YaST to add the NVIDIA repository and install the recommended driver.
openSUSE's official documentation provides the repository for each release.

1. Open **YaST → Software → Add-on Products** or use **My Computer → Hardware →
   Graphics Card**.
2. Follow the wizard for your NVIDIA card.
3. Reboot when prompted.

You can also install recommended packages after the repository is configured:

```bash
sudo zypper install-new-recommends
```

Read the official openSUSE NVIDIA guide for your exact release before adding a
repository manually:
[openSUSE NVIDIA driver guide](https://en.opensuse.org/SDB:NVIDIA_drivers)

## Configuration

### Check whether you are using Wayland or X11

```bash
echo "$XDG_SESSION_TYPE"
```

The output is usually `wayland` or `x11`. NVIDIA support on Wayland has improved,
but some applications and older drivers work better on X11. Choose the session
type at the login screen if both are offered.

Do not switch session types while troubleshooting an unrelated problem. Change
one thing at a time.

### Laptop graphics switching (PRIME)

Laptops with both Intel/AMD and NVIDIA graphics may use PRIME to switch between
the integrated and discrete GPU. Configuration differs by distribution and
driver version.

- Ubuntu and Linux Mint: use the NVIDIA settings tool or the distribution's
  PRIME documentation.
- Fedora: read the RPM Fusion PRIME guide.
- Arch Linux: read the ArchWiki PRIME section.
- openSUSE: read the openSUSE NVIDIA guide.

Do not force the discrete GPU on battery power unless you need it; it increases
power use and heat.

### Secure Boot and kernel modules

If Secure Boot is enabled and the driver is not loading, check the kernel
messages:

```bash
sudo dmesg | grep -i -E 'nvidia|secure|module'
```

Do not disable Secure Boot as a first step. Enroll the key using your
distribution's official procedure. Keep the enrollment password until the
process is complete.

## Verification

Check the kernel driver:

```bash
lspci -k | grep -A 3 -E 'VGA|3D'
```

For NVIDIA, the `Kernel driver in use` line should show `nvidia`. For Intel or
AMD, it should show `i915`, `xe`, or `amdgpu`.

Check the NVIDIA tool if installed:

```bash
nvidia-smi
```

This prints the driver version, GPU model, memory use, and running processes. If
the command is missing, the NVIDIA driver is probably not installed.

Check OpenGL:

```bash
glxinfo -B
```

This prints the renderer and OpenGL version. Install `mesa-utils` or the
equivalent package if the command is missing.

Check the session type:

```bash
echo "$XDG_SESSION_TYPE"
```

Restart the computer and repeat the checks. A driver that works before a reboot
but fails afterwards is not fully configured.

## Troubleshooting

### The screen is black after installing the NVIDIA driver

1. Wait a minute; the driver may still be loading.
2. Restart and choose an older kernel or recovery mode from the GRUB menu.
3. Remove the NVIDIA driver and return to the open-source driver.
4. Read your distribution's official NVIDIA troubleshooting page.

Do not reinstall the whole system before trying recovery mode.

### `nouveau` is still in use

The open-source `nouveau` driver may still be loaded. Check:

```bash
lsmod | grep nouveau
```

Follow your distribution's official instructions to blacklist `nouveau` and
rebuild the initramfs. Do not copy a blacklist file from an unrelated
distribution.

### Secure Boot rejects the NVIDIA module

- Confirm that Secure Boot is enabled.
- Enroll the MOK using your distribution's procedure.
- Check `dmesg` for a module verification error.
- Do not disable Secure Boot unless you understand the security and BitLocker
  implications.

### `nvidia-smi` fails or reports no devices

- Confirm that the NVIDIA driver package is installed.
- Check `lspci -k` for the driver in use.
- Reboot after a kernel or driver update.
- On a laptop, the discrete GPU may be powered down until an application uses
  it.

### External monitor does not work

- Check the cable and port.
- Try the monitor on another computer.
- On NVIDIA laptops, check the PRIME configuration.
- On Wayland, test an X11 session to see whether the problem is session-specific.

### The desktop is slow after a driver update

- Check whether the system is using software rendering:
  ```bash
  glxinfo -B
  ```
  If the renderer is `llvmpipe`, the GPU driver is not in use.
- Check `dmesg` for driver errors.
- Boot an older kernel and compare.
- Roll back the driver using your distribution's package manager.

## Uninstall or Restore

### Remove the driver on Ubuntu and Linux Mint

Open **Software & Updates → Additional Drivers** and select the open-source
driver, or remove the NVIDIA packages:

```bash
sudo apt purge '^nvidia-.*'
sudo apt autoremove
```

> **Warning:** Broad package patterns can remove more than you expect. Review
> the list before confirming. If you are unsure, use the Additional Drivers GUI
> instead.

Reboot after removing the driver.

### Remove the driver on Fedora

```bash
sudo dnf remove akmod-nvidia 'xorg-x11-drv-nvidia*' 'kmod-nvidia*'
```

Wait for the removal to finish, then reboot. If you no longer need RPM Fusion,
disable or remove the repositories separately.

### Remove the driver on Arch Linux

```bash
sudo pacman -Rns nvidia nvidia-utils
```

Use `nvidia-lts` or `nvidia-dkms` in place of `nvidia` if that is what you
installed. Reboot after removal.

### Remove the driver on openSUSE

Use YaST to remove the NVIDIA driver packages, or:

```bash
sudo zypper remove nvidia-driver-G06-kmp-default
```

The exact package name depends on your release and driver branch. Check the
installed packages first:

```bash
zypper search --installed-only nvidia
```

Then remove the matching package. Reboot after removal.

### Restore the desktop

If the desktop does not start, boot recovery mode or an older kernel, then remove
the driver. If you cannot reach a shell, use a live USB to back up your files
before repairing the system. See
[Backup and restore](../maintenance/backup-and-restore.md).

## References

- [Ubuntu NVIDIA driver installation](https://ubuntu.com/server/docs/nvidia-drivers-installation)
- [RPM Fusion NVIDIA how-to](https://rpmfusion.org/Howto/NVIDIA)
- [ArchWiki: NVIDIA](https://wiki.archlinux.org/title/NVIDIA)
- [ArchWiki: AMDGPU](https://wiki.archlinux.org/title/AMDGPU)
- [ArchWiki: Intel graphics](https://wiki.archlinux.org/title/Intel_graphics)
- [openSUSE NVIDIA driver guide](https://en.opensuse.org/SDB:NVIDIA_drivers)
- [Mesa documentation](https://docs.mesa3d.org/)
- [NVIDIA Linux driver downloads](https://www.nvidia.com/Download/index.aspx)
