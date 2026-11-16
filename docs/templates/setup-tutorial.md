# Setup Tutorial Template

Use this structure for practical tutorials that install or configure software.
Keep the page focused on one task. Replace the guidance in italics with real
content and delete the italics before submitting.

> **Safety:** Do not include a command you cannot explain. Do not claim that
> instructions were tested unless they were tested on the stated distribution
> and version.

## Metadata

At the top of the page, include:

- **Supported systems:** distributions and versions known to work
- **Difficulty:** beginner, intermediate, or advanced
- **Terminal required:** yes or no
- **Last reviewed:** month and year, or "not yet reviewed"

## Overview

Explain what the user will set up and why it is useful. State what is out of
scope. If more than one method exists, say which method this tutorial uses.

## Requirements

List everything the user needs before starting:

- supported Linux distribution and version
- hardware requirements
- internet connection and free disk space
- an account with `sudo` access, if needed
- files or account details the user must prepare
- a backup, when the task can change disks, boot settings, or user data

Add a warning before any step that can cause data loss.

## Installation

Give numbered, testable steps. For each command:

1. Explain what it does in simple English.
2. Show the command in a fenced code block.
3. Show the expected result or output where useful.

Prefer the distribution's official package manager or official vendor
instructions. Use one primary method and put alternatives in a short note.

## Configuration

Explain the settings the user must change after installation. Include:

- configuration file paths and what each option does
- GUI menu paths when a desktop tool is available
- required restarts, logouts, or service reloads
- security-related choices and their trade-offs

## Verification

Give commands or GUI checks that prove the setup works. Include the expected
output and at least one check that survives a reboot.

## Troubleshooting

List common problems with symptoms and safe fixes. Start with non-destructive
checks. Include a warning before any fix that deletes files, changes boot
settings, or disables a security feature.

## Uninstall or Restore

Explain how to undo the tutorial safely:

- remove installed packages or applications
- revert configuration changes
- restore data from a backup
- restore firmware or boot settings, if they were changed

If the action cannot be reversed easily, say so before the installation
section as well.

## References

Link to official documentation, release notes, and vendor guides. Do not copy
long passages from other sites. Use relative links for other pages in this
repository and full URLs for external documentation.

## Checklist

Before opening a pull request:

- [ ] The page follows the eight sections above.
- [ ] Commands are explained before they appear.
- [ ] Risky commands have a clear warning.
- [ ] Supported versions are stated.
- [ ] Relative links resolve.
- [ ] Nothing is described as verified unless it was tested.
