# Desktop Setup — Ubuntu

**Supported version:** Ubuntu Desktop 26.04 LTS with GNOME

**Difficulty:** Beginner

**Terminal required:** Only for installing the optional desktop tools.

**Last reviewed:** Not yet reviewed on live systems.

**Learning path:** ← [Initial Setup](./04-initial-setup.md) · [Ubuntu overview](./README.md) · [Troubleshooting](./06-troubleshooting.md) →

## Overview

This tutorial configures the Ubuntu desktop for daily use. You will set the
display, appearance, keyboard, power options, and a few safe GNOME extensions.

Ubuntu Desktop uses **GNOME** with Ubuntu's own theme and layout. This tutorial
does not replace GNOME with another desktop. To compare desktops, see
[Linux desktop environments](../guides/desktop-environments.md).

## Requirements

- Ubuntu Desktop installed and updated. See [Initial Setup](./04-initial-setup.md).
- A working display and input devices.
- An internet connection for optional tools and extensions.
- `sudo` access for package installation.

## Installation

### Step 1: Open the main settings

Open the **Settings** application from the application menu, or click the
system menu in the top-right corner and choose **Settings**.

Review these panels first:

| Panel | What to check |
|---|---|
| **Displays** | Resolution, refresh rate, scale, and monitor arrangement |
| **Sound** | Output device, input device, and volume |
| **Power** | Screen blank timeout and automatic suspend |
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

### Step 3: Set appearance and wallpaper

1. Open **Settings → Appearance**.
2. Choose light or dark style.
3. Choose an accent color and wallpaper.
4. Open **Settings → Accessibility** if you need larger text, a screen reader, or
   other assistance.

### Step 4: Install GNOME Tweaks and Extension Manager (optional)

GNOME Tweaks exposes settings that are not in the main Settings app. Extension
Manager lets you install and remove GNOME extensions.

```bash
sudo apt install gnome-tweaks gnome-shell-extension-manager
```

- `gnome-tweaks` adds options for fonts, title bars, and window controls.
- `gnome-shell-extension-manager` is a graphical tool for managing extensions.

Open **Tweaks** and **Extension Manager** from the application menu after
installation.

### Step 5: Add a few extensions (optional)

GNOME extensions add features such as application indicators, clipboard history,
or window tiling.

1. Open **Extension Manager**.
2. Browse the built-in catalog, or open the official
   [GNOME Extensions](https://extensions.gnome.org/) website.
3. Read the extension's description and supported GNOME version.
4. Install one extension at a time.
5. Test the desktop after each installation.

> **Safety:** Extensions run inside your desktop session. Install only extensions
> you trust, and remove any extension that causes crashes or login problems.
> Ubuntu may limit extension support after a GNOME upgrade until the extension is
> updated.

### Step 6: Configure the terminal (optional)

Ubuntu includes GNOME Terminal. Open it with `Ctrl` + `Alt` + `T`.

Useful terminal settings:

1. Open **Terminal → Preferences**.
2. Choose a readable font size.
3. Enable a visible cursor and a suitable color scheme.
4. Review the shortcut for copying and pasting. In GNOME Terminal, copy is
   usually `Ctrl` + `Shift` + `C` and paste is `Ctrl` + `Shift` + `V`.

For terminal basics, see [Basic Linux commands](../guides/basic-commands.md).

## Configuration

### Keyboard shortcuts

Open **Settings → Keyboard → Keyboard Shortcuts** and review the shortcuts you
use most:

- switch applications
- switch windows
- open a terminal
- lock the screen
- take a screenshot

You can add a custom shortcut, but avoid replacing a shortcut you rely on.

### Power and battery

Open **Settings → Power**:

- Set **Screen Blank** to a short timeout to save power.
- Set **Automatic Suspend** based on whether you want the computer to sleep.
- On a laptop, check the battery percentage and charging status.

If suspend does not resume correctly, see
[Ubuntu troubleshooting](./06-troubleshooting.md).

### Night Light and color

Open **Settings → Displays → Night Light** to reduce blue light in the evening.
This is a comfort setting, not a substitute for good lighting.

### Desktop icons and dock

Ubuntu's dock and desktop icon settings are available in **Settings → Ubuntu
Desktop** on releases that include that panel. The exact location can change
between Ubuntu versions.

- Move the dock to the left or bottom if your release allows it.
- Choose whether to show mounted drives on the desktop.
- Keep the dock small enough that it does not cover application controls.

## Verification

After configuring the desktop:

- [ ] The display uses the intended resolution and refresh rate.
- [ ] The correct monitor arrangement survives a restart.
- [ ] Sound plays through the intended output device.
- [ ] The keyboard layout matches your physical keyboard.
- [ ] The screen locks when you leave it, if you enabled locking.
- [ ] GNOME Tweaks and Extension Manager open if you installed them.
- [ ] Any installed extension still works after logging out and back in.

Check the GNOME version:

```bash
gnome-shell --version
```

This prints the running GNOME Shell version. It is useful when an extension
reports a compatibility problem.

## Troubleshooting

### The display settings revert after I click Apply

The selected mode may not be supported. Choose the recommended resolution and
refresh rate, or try a lower refresh rate. If the screen remains black, wait for
the automatic revert or restart in recovery mode.

### The desktop looks broken after installing an extension

1. Log out and log back in.
2. Open **Extension Manager** and disable the newest extension.
3. If the desktop does not load, switch to a text console with
   `Ctrl` + `Alt` + `F3`, sign in, and ask for help before deleting files.

Do not remove GNOME packages to fix an extension. Remove the extension instead.

### Text is blurry

Fractional scaling can blur applications that do not support it. Try 100% or
200% scaling, choose a resolution that matches your monitor, or keep fractional
scaling and accept that some applications may look soft.

### GNOME Tweaks does not show a setting

Some settings depend on the GNOME version or on an extension. Check the GNOME
release notes and the extension's documentation. Not every older tweak is
available in newer releases.

### The dock or top bar disappeared

Log out and log back in. If the problem started after installing an extension,
disable that extension from **Extension Manager** or from a text console. If it
started after a system update, check
[Ubuntu troubleshooting](./06-troubleshooting.md).

## Uninstall or Restore

### Remove optional desktop tools

```bash
sudo apt remove gnome-tweaks gnome-shell-extension-manager
```

This removes the tools, not your desktop. Extensions installed through Extension
Manager must be removed in the app before or after removing the manager.

### Disable an extension

1. Open **Extension Manager**.
2. Select the extension.
3. Choose **Disable** or **Remove**.

If the desktop is unusable, use a text console and remove the extension's folder
under `~/.local/share/gnome-shell/extensions/`. Only delete the folder whose
identifier matches the extension you installed.

### Reset a single setting

Prefer changing one setting back in **Settings** or **Tweaks**. Avoid broad
commands that reset all GNOME settings unless you have a backup and understand
what they remove.

## References

- [Ubuntu Desktop documentation](https://ubuntu.com/desktop/docs/)
- [GNOME Help](https://help.gnome.org/)
- [GNOME Extensions](https://extensions.gnome.org/)
- [GNOME Tweaks](https://gitlab.gnome.org/GNOME/gnome-tweaks)
- [Ubuntu community documentation](https://help.ubuntu.com/community/)
- [Linux desktop environments](../guides/desktop-environments.md)

**Next:** [Troubleshoot common problems](./06-troubleshooting.md)
