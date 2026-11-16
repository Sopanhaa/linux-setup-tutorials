# Desktop Setup — Fedora

**Supported version:** Fedora Workstation 45 with GNOME 51

**Difficulty:** Beginner

**Terminal required:** Only for installing optional desktop tools.

**Last reviewed:** Not yet reviewed on live systems.

**Learning path:** ← [Initial Setup](./04-initial-setup.md) · [Fedora overview](./README.md) · [Troubleshooting](./06-troubleshooting.md) →

## Overview

This tutorial configures the Fedora Workstation desktop for daily use. You will
set the display, appearance, sound, power options, keyboard shortcuts, terminal,
and a few safe GNOME extensions.

Fedora Workstation 45 uses **GNOME 51** with a **Wayland** session. This tutorial
does not replace GNOME with another desktop. To compare desktops, see
[Linux desktop environments](../guides/desktop-environments.md).

## Requirements

- Fedora Workstation 45 installed and updated. See
  [Initial Setup](./04-initial-setup.md).
- A working display and input devices.
- An internet connection for optional tools and extensions.
- `sudo` access for package installation.

## Installation

### Step 1: Open the main settings

Open **Settings** from the application menu, or click the system menu in the
top-right corner and choose **Settings**.

Review these panels first:

| Panel | What to check |
|---|---|
| **Displays** | Resolution, refresh rate, scale, and monitor arrangement |
| **Sound** | Output device, input device, and volume |
| **Power** | Power mode, screen blank timeout, and automatic suspend |
| **Keyboard** | Layout and shortcuts |
| **Network** | Wi-Fi or wired connection |
| **Date & Time** | Time zone and automatic time |

### Step 2: Set the display

1. Open **Settings → Displays**.
2. Choose the recommended resolution and refresh rate.
3. If text is too small, try a scaling option. Fractional scaling can make some
   applications blurry; test your main applications before keeping it.
4. If you use two monitors, drag them into the same physical arrangement as your
   desk.
5. Click **Apply** and confirm that the display works. If the screen goes black,
   wait; the settings revert automatically if you do not confirm.

Fedora Workstation 45 uses Wayland. Some older applications may not scale
perfectly under fractional scaling. That is an application limitation, not
necessarily a broken driver.

### Step 3: Set appearance and wallpaper

1. Open **Settings → Appearance**.
2. Choose light or dark style.
3. Choose an accent color and wallpaper.
4. Open **Settings → Accessibility** if you need larger text, a screen reader, or
   other assistance.

### Step 4: Set sound and power options

Open **Settings → Sound** and choose the correct output and input devices. Test
the speakers and microphone.

Open **Settings → Power**:

- Set **Screen Blank** to a short timeout to save power.
- Set **Automatic Suspend** based on whether you want the computer to sleep.
- On a laptop, choose a power mode that suits the task. **Balanced** is a good
  default; **Performance** uses more battery.

You can also check the power profile in a terminal:

```bash
powerprofilesctl get
powerprofilesctl set balanced
```

- `powerprofilesctl get` prints the current profile.
- `powerprofilesctl set balanced` switches to the balanced profile.

Replace `balanced` with `performance` or `power-saver` if those profiles are
available on your system.

### Step 5: Review keyboard shortcuts

Open **Settings → Keyboard → Keyboard Shortcuts** and review the shortcuts you
use most:

- switch applications
- switch windows
- open a terminal
- lock the screen
- take a screenshot

You can add a custom shortcut, but avoid replacing a shortcut you rely on.

### Step 6: Install GNOME Tweaks and the Extensions app (optional)

GNOME Tweaks exposes settings that are not in the main Settings app. The GNOME
Extensions app lets you enable, disable, and configure installed extensions.

```bash
sudo dnf install gnome-tweaks gnome-extensions-app
```

- `gnome-tweaks` adds options for fonts, title bars, and window controls.
- `gnome-extensions-app` is the official GNOME app for managing extensions.

Open **Tweaks** and **Extensions** from the application menu after installation.

### Step 7: Add a few extensions (optional)

GNOME extensions add features such as application indicators, clipboard history,
or window tiling.

1. Open **Extensions** to see what is already installed.
2. To browse new extensions, open the official
   [GNOME Extensions](https://extensions.gnome.org/) website, or install
   Extension Manager from Flathub:
   ```bash
   flatpak install flathub com.mattjakeman.ExtensionManager
   ```
3. Read the extension's description and confirm that it supports **GNOME 51**.
4. Install one extension at a time.
5. Log out and back in so GNOME loads the new extension.
6. Test the desktop after each installation.

> **Safety:** Extensions run inside your desktop session. Install only extensions
> you trust, and remove any extension that causes crashes or login problems. A
> GNOME upgrade can break an extension until its author updates it.

Useful commands for installed extensions:

```bash
gnome-extensions list
gnome-extensions info EXTENSION-UUID
gnome-extensions disable EXTENSION-UUID
gnome-extensions enable EXTENSION-UUID
```

Replace `EXTENSION-UUID` with the identifier from `gnome-extensions list`.
`info` shows whether the extension is enabled and active.

### Step 8: Configure the terminal (optional)

Fedora Workstation 45 includes **Ptyxis** as the default terminal. Open it from
the application menu or search for **Terminal** in Activities.

Check the installed terminal:

```bash
rpm -q ptyxis
```

Open **Terminal → Preferences** to choose a readable font size and review
keyboard shortcuts. Ptyxis uses `Ctrl` + `Shift` + `C` and `Ctrl` + `Shift` + `V`
for copy and paste by default; check the preferences if your version differs.

For terminal basics, see [Basic Linux commands](../guides/basic-commands.md).

## Configuration

### Night Light and color

Open **Settings → Displays → Night Light** to reduce blue light in the evening.
This is a comfort setting, not a substitute for good lighting.

### Default applications

Open **Settings → Default Applications** and choose your preferred browser,
email client, and media player. Fedora Workstation includes Firefox and
LibreOffice.

### Software and updates

Use **Software** to browse applications and install updates. Fedora Workstation
uses Flatpak for many desktop applications and DNF for system packages.

Do not mix system packages from random third-party sites with Flatpak versions
of the same application unless you understand the difference. Pick one source per
application.

### Desktop icons and extensions

GNOME does not show desktop icons by default. If you want them, install an
extension that provides them and check that it supports GNOME 51. Remove it if it
causes instability.

### Terminal profile

In Ptyxis preferences, you can create profiles with different colors, fonts, and
container settings. Keep at least one profile with a readable font and a visible
cursor.

## Verification

After configuring the desktop:

- [ ] The display uses the intended resolution and refresh rate.
- [ ] The correct monitor arrangement survives a restart.
- [ ] Sound plays through the intended output device.
- [ ] The keyboard layout matches your physical keyboard.
- [ ] The screen locks when you leave it, if you enabled locking.
- [ ] GNOME Tweaks and Extensions open if you installed them.
- [ ] Any installed extension still works after logging out and back in.

Check the GNOME version and session type:

```bash
gnome-shell --version
echo "$XDG_SESSION_TYPE"
```

`gnome-shell --version` prints the GNOME Shell version. The session type should
usually print `wayland` on Fedora Workstation 45.

Check an extension:

```bash
gnome-extensions list
```

This lists installed extensions. An extension that does not appear may not be
installed for your user or may be incompatible with GNOME 51.

## Troubleshooting

### The desktop looks broken after installing an extension

1. Log out and log back in.
2. Open **Extensions** and disable the newest extension.
3. If the desktop does not load, switch to a text console with
   `Ctrl` + `Alt` + `F3`, sign in, and disable the extension with:
   ```bash
   gnome-extensions disable EXTENSION-UUID
   ```
4. If you cannot identify the extension, ask for help before deleting files.

Do not remove GNOME packages to fix an extension. Remove the extension instead.

### An extension says it is out of date

The extension does not support GNOME 51. Check the extension's page for a newer
release. If there is none, disable or remove it. Do not force an incompatible
extension to load.

### Text is blurry

Fractional scaling can blur applications that do not support it. Try 100% or
200% scaling, choose a resolution that matches your monitor, or keep fractional
scaling and accept that some applications may look soft.

### GNOME Tweaks does not show a setting

Some settings depend on the GNOME version or on an extension. Check the GNOME
release notes and the extension's documentation. Not every older tweak is
available in newer releases.

### The terminal shortcut does not work

Fedora Workstation uses Ptyxis by default. Open **Terminal → Preferences →
Shortcuts** and check the copy and paste shortcuts. Older guides that mention
GNOME Terminal may not match your installed terminal.

### The system uses software rendering

Check the renderer:

```bash
glxinfo -B
```

If the renderer is `llvmpipe`, the GPU driver is not in use. Install `glx-utils`
if the command is missing, then check your graphics driver. See
[Graphics drivers](../guides/graphics-drivers.md#troubleshooting).

### The desktop does not start after a driver change

- Boot an older kernel from the GRUB menu or use recovery mode.
- Remove the recently installed driver.
- Read [Graphics drivers](../guides/graphics-drivers.md#troubleshooting).

## Uninstall or Restore

### Remove optional desktop tools

```bash
sudo dnf remove gnome-tweaks gnome-extensions-app
```

This removes the tools, not the GNOME desktop. Extensions installed through the
Extensions app or Extension Manager must be removed in the app before or after
removing the tools.

### Remove Extension Manager

```bash
flatpak uninstall com.mattjakeman.ExtensionManager
flatpak uninstall --unused
```

### Disable an extension

```bash
gnome-extensions disable EXTENSION-UUID
```

To remove an extension installed for your user, use the Extensions app or
Extension Manager. Do not delete extension folders unless the app cannot remove
them and you know the exact identifier.

### Reset a single setting

Prefer changing one setting back in **Settings** or **Tweaks**. Avoid broad
commands that reset all GNOME settings unless you have a backup and understand
what they remove.

## References

- [Fedora Workstation documentation](https://docs.fedoraproject.org/en-US/workstation-docs/)
- [GNOME Help](https://help.gnome.org/)
- [GNOME Extensions](https://extensions.gnome.org/)
- [Fedora Quick Docs: GNOME Shell extensions](https://docs.fedoraproject.org/en-US/quick-docs/gnome-shell-extensions)
- [Fedora Magazine: Customize Workstation with Extension Manager](https://fedoramagazine.org/customize-fedora-linux-workstation-with-extension-manager/)
- [Ptyxis terminal](https://gitlab.gnome.org/chergert/ptyxis)
- [Linux desktop environments](../guides/desktop-environments.md)

**Next:** [Troubleshoot common problems](./06-troubleshooting.md)
