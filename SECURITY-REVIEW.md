# Security Review — Linux Setup Tutorials

**Review date:** 2026-11-16

**Repository:** `Sopanhaa/linux-setup-tutorials`

**Scope:** Documentation, Markdown instructions, repository automation, scripts,
and secrets in the current tree and Git history.

**Reviewer stance:** Senior Linux security and documentation safety review.

## Executive summary

This repository is a documentation-only project. It contains no shell scripts,
no executable files, no application code, and no GitHub Actions workflows. No
hardcoded credentials or private keys were found in the current tree or in the
available Git history.

| Category | Result |
|---|---|
| Confirmed exploitable code or CI vulnerabilities | **0** |
| Confirmed unsafe documentation instructions | **9** |
| Potential supply-chain items requiring validation | **3** |
| Rejected candidates or adequately warned operations | **13** |

The main risk is not remote exploitation of this repository. It is that a
beginner may copy privileged commands from the tutorials and expose a service,
remove packages they still need, or make a system unbootable.

> **No target-controlled code or destructive commands were executed during this
> review.** Commands were inspected statically. No package installations,
> firewall changes, driver removals, disk operations, or bootloader changes were
> run.

## Skill availability

The Cloudflare `security-audit` skill was **not installed** in this environment.
I retrieved its public README to align this report with its terminology:
`confirmed`, `needs_validation`, and `rejected`.

The skill requires an OS-enforced sandbox before executing target-controlled
code. This repository has no target-controlled code to execute, so no sandbox
execution was needed. The review stays at the documentation and supply-chain
level.

## Remediation status in the working tree

These changes were made after the review and are **not committed**. The findings
below describe the reviewed revision; current line numbers for fixed items are in
this table.

| Finding | Status | Current location |
|---|---|---|
| F-01 — Firewall port open to all sources | **Fixed** | `docs/fedora/06-troubleshooting.md:326-350` |
| F-02 — Bootloader regeneration without backup | **Fixed** | `docs/fedora/03-installation.md:294-324`, `docs/fedora/06-troubleshooting.md:372-405` |
| F-05 — DNF history undo without preview | **Fixed** | `docs/fedora/06-troubleshooting.md:249-260` |
| F-08 — USB tools not verified | **Partially fixed** | Fedora Media Writer verification added at `docs/fedora/02-download.md:132-134`; Rufus/Ventoy remain open in `docs/guides/bootable-usb.md` |
| F-09 — Repository disable could stop updates | **Fixed** | `docs/fedora/06-troubleshooting.md:193-207` |
| PV-01 — Fedora key fingerprint | **Addressed; manual verification remains** | `docs/fedora/02-download.md:105-116` |
| F-03 — Broad NVIDIA removal | Open | `docs/guides/graphics-drivers.md` |
| F-04 — Broad Arch orphan removal | Open | `docs/guides/system-maintenance.md` |
| F-06 — Firewall disable as uninstall step | Open | `docs/ubuntu/04-initial-setup.md` |
| F-07 — `apt purge` warning | Open | `docs/ubuntu/06-troubleshooting.md` |
| F-08 — Rufus/Ventoy verification | Open | `docs/guides/bootable-usb.md` |
| PV-02 — Live firewall-zone validation | Open | Requires live testing |
| PV-03 — Extension/Flatpak permission review | Open | Requires validation |

## Method and coverage

### Files inspected

- `README.md`
- `CONTRIBUTING.md` (present during the review; restored afterward in a shorter
  form)
- `ROADMAP.md`
- All distribution tutorials under `docs/ubuntu/`, `docs/fedora/`,
  `docs/arch-linux/`, `docs/debian/`, `docs/linux-mint/`, and `docs/opensuse/`
- All guides under `docs/guides/`
- Hubs under `docs/system-configuration/`, `docs/troubleshooting/`, and
  `docs/maintenance/`
- `docs/categories/`
- `docs/templates/setup-tutorial.md`

### Repository checks

- `git ls-files` — 65 Markdown files, no scripts or workflows.
- Searched for `.sh`, `.bash`, `.zsh`, `.py`, `.yml`, `.yaml`, Dockerfiles,
  Makefiles, systemd units, and `.github/` — none found.
- Searched for executable files and symlinks — none found.
- Searched for hardcoded secrets, tokens, private keys, and cloud credentials in
  the current tree and all commits — no matches.
- Searched for `curl | bash`, `wget | sh`, `eval`, `--nogpgcheck`,
  `--no-check-certificate`, `rm -rf /`, `mkfs`, `dd`, `wipefs`, `setenforce 0`,
  and `systemctl disable firewalld`.
- Reviewed all `sudo`, package-removal, firewall, bootloader, driver, and
  destructive-file commands in context.
- Checked that every external download link uses HTTPS. No `http://` links were
  found.

## Confirmed vulnerabilities

**None.**

No confirmed vulnerability has a reproducible source-to-impact path in this
repository. There is no code execution surface, no CI workflow, no shell script,
no credential store, and no application endpoint.

The absence of confirmed vulnerabilities is based on:

- no shell scripts or executable files in the repository;
- no GitHub Actions workflows;
- no secrets found in current files or Git history;
- no download-and-execute pipeline;
- no insecure HTTP links;
- no instructions to disable SELinux, firewalld, or Secure Boot as a first step.

The items below are **confirmed unsafe documentation instructions**, not
confirmed software vulnerabilities.

## Confirmed unsafe documentation instructions

### F-01 — Firewall port opened for all sources

**File:** `docs/fedora/06-troubleshooting.md:326` (reviewed revision: line 301)

**Severity:** Medium

**Status:** Fixed in the working tree.

**Instruction:**

```bash
sudo firewall-cmd --add-port=8080/tcp
```

**Why it is unsafe:** The command adds the port to the default zone for all
sources. On a Fedora Workstation the default zone is often
`FedoraWorkstation`, which already allows high ports. On a system using the
`public` zone, this exposes port 8080 to every host that can reach the machine.
The surrounding text does not require a zone, source restriction, or immediate
cleanup.

**Recommended correction:**

- Specify the intended zone with `--zone=`.
- Restrict the source with `--add-rich-rule` or use a named service.
- Show the removal command and verification:

```bash
sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.0/24" port port="8080" protocol="tcp" accept'
sudo firewall-cmd --list-all --zone=public
sudo firewall-cmd --zone=public --remove-rich-rule='rule family="ipv4" source address="192.168.1.0/24" port port="8080" protocol="tcp" accept'
```

- Add a warning that opening a port exposes a service to the network and that
  `--permanent` should only be used after testing.

**Official reference:** [firewalld firewall-cmd manual](https://firewalld.org/documentation/man-pages/firewall-cmd) · [Fedora firewalld documentation](https://docs.fedoraproject.org/en-US/quick-docs/firewalld/)

---

### F-02 — Bootloader regeneration without backup or path verification

**Files and lines:**

- `docs/fedora/03-installation.md:294-308` (reviewed revision: line 290)
- `docs/fedora/06-troubleshooting.md:372-386` (reviewed revision: line 319)
- `docs/ubuntu/03-installation.md:249-250`

**Severity:** Medium for Fedora, Low for Ubuntu

**Status:** Fedora sections fixed in the working tree. Ubuntu `update-grub` warning
remains open.

**Instructions:**

```bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

```bash
sudo os-prober
sudo update-grub
```

**Why it is unsafe:** These commands rewrite bootloader configuration. A wrong
path, interrupted write, or incorrect UEFI/BIOS assumption can leave the system
unbootable. The Fedora text says the UEFI path "may be"
`/boot/efi/EFI/fedora/grub.cfg`, but the command still writes the BIOS-style
path first. There is no instruction to back up the existing configuration or to
have a live USB ready.

**Recommended correction:**

- Tell the user to identify UEFI or BIOS mode first.
- Back up the current GRUB configuration before regenerating it.
- Show the correct output path for the detected mode.
- Add a warning that a live USB is required for recovery if boot fails.
- Prefer the distribution's official bootloader tool where available.

Example warning to add:

> **Bootloader warning:** This rewrites the GRUB configuration. Back up the
> current file, confirm whether the system uses UEFI or BIOS, and keep a live USB
> available. A wrong path can make the system unbootable.

**Official reference:** [Fedora GRUB2 bootloader documentation](https://docs.fedoraproject.org/en-US/quick-docs/grub2-bootloader/) · [GNU GRUB manual](https://www.gnu.org/software/grub/manual/grub/)

---

### F-03 — Broad NVIDIA package removal without consistent warnings

**Files and lines:**

- `docs/guides/graphics-drivers.md:352`
- `docs/guides/graphics-drivers.md:365`
- `docs/guides/graphics-drivers.md:374`

**Severity:** Medium

**Instructions:**

```bash
sudo apt purge '^nvidia-.*'
```

```bash
sudo dnf remove akmod-nvidia 'xorg-x11-drv-nvidia*' 'kmod-nvidia*'
```

```bash
sudo pacman -Rns nvidia nvidia-utils
```

**Why it is unsafe:** These commands can remove more than the intended driver,
including helper packages, settings tools, or dependencies used by another GPU
stack. Removing the active graphics driver can leave the desktop unable to start.
Only the Ubuntu section has a broad-pattern warning; the Fedora and Arch
sections do not.

**Recommended correction:**

- Add the same warning to every distribution's removal section.
- Tell users to list installed NVIDIA packages before removing anything:
  - Fedora: `rpm -qa | grep -i nvidia`
  - Arch: `pacman -Qs nvidia`
  - Ubuntu: `apt list --installed | grep -i nvidia`
- Recommend the distribution's driver GUI or package-specific removal first.
- Tell users to have a text console, older kernel, or live USB ready.
- Note that after removal the desktop may fall back to software rendering.

**Official reference:** [RPM Fusion NVIDIA guide](https://rpmfusion.org/Howto/NVIDIA) · [ArchWiki NVIDIA](https://wiki.archlinux.org/title/NVIDIA) · [Ubuntu NVIDIA driver installation](https://ubuntu.com/server/docs/nvidia-drivers-installation)

---

### F-04 — Removing every orphaned package in one command

**File:** `docs/guides/system-maintenance.md:130`

**Severity:** Medium

**Instruction:**

```bash
sudo pacman -Rns $(pacman -Qtdq)
```

**Why it is unsafe:** The command removes every package that the package manager
currently considers unneeded. A package can still be useful to the user even when
no installed package depends on it. The command substitution also makes the full
removal list hard to review before execution. A warning exists, but the command
is still presented as a single copyable action.

**Recommended correction:**

- Keep the read-only listing step.
- Show a dry-run or preview command before removal.
- Recommend removing packages one at a time after checking optional
  dependencies.
- If the one-line form remains, show it only after saving the list and with a
  clear warning.

Example safer flow:

```bash
pacman -Qtdq
sudo pacman -Rns --print $(pacman -Qtdq)
```

**Official reference:** [ArchWiki pacman](https://wiki.archlinux.org/title/Pacman)

---

### F-05 — Undoing a DNF transaction without a preview step

**File:** `docs/fedora/06-troubleshooting.md:249-256` (reviewed revision: line 238)

**Severity:** Low

**Status:** Fixed in the working tree with a preview step.

**Instruction:**

```bash
sudo dnf history undo ID
```

**Why it is unsafe:** Undoing a transaction can downgrade or remove packages that
later work depends on. Old package versions may no longer be available, causing a
partial or failed transaction. A warning exists, but there is no preview command
and no explicit instruction to inspect the transaction first.

**Recommended correction:**

- Add `sudo dnf history info ID` before the undo.
- Recommend `--assumeno` or an interactive review of the transaction summary.
- Explain that configuration files and user data are not restored by DNF.
- Tell users to back up important configuration before large rollbacks.

**Official reference:** [DNF5 history command](https://dnf5.readthedocs.io/en/stable/commands/history.8.html) · [Fedora DNF documentation](https://docs.fedoraproject.org/en-US/quick-docs/dnf/)

---

### F-06 — Firewall disable shown as a normal uninstall step

**File:** `docs/ubuntu/04-initial-setup.md:353`

**Severity:** Low

**Instruction:**

```bash
sudo ufw disable
```

**Why it is unsafe:** Disabling the firewall is a security regression. Even
though the page says to do it only if the user understands the risk, placing it
under **Uninstall or Restore** normalizes turning off a security control. A
beginner may disable the firewall and never re-enable it.

**Recommended correction:**

- Remove the command from the general uninstall section.
- If it must stay for troubleshooting, make it explicitly temporary:

```bash
sudo ufw disable
# Test the specific problem.
sudo ufw enable
sudo ufw status
```

- Recommend adding a targeted rule instead of disabling the firewall.
- Mention the SSH lockout risk before any disable/enable operation.

**Official reference:** [Ubuntu UFW documentation](https://help.ubuntu.com/community/UFW)

---

### F-07 — Purge command without configuration and dependency warning

**File:** `docs/ubuntu/06-troubleshooting.md:273`

**Severity:** Low

**Instruction:**

```bash
sudo apt purge package-name
```

**Why it is unsafe:** `apt purge` removes system-wide configuration in addition
to the package. It can also remove packages that depend on it. The page says
purge does not remove personal files, but it does not warn about service
configuration, credentials stored in package configuration, or dependency
removal.

**Recommended correction:**

- Default to `sudo apt remove package-name` for troubleshooting.
- Recommend `purge` only when the user intends to remove configuration.
- Tell users to back up configuration files and review the transaction list.
- Explain that reinstall may be required after purging a service package.

**Official reference:** [Debian apt-get manual](https://manpages.debian.org/stable/apt/apt-get.8.en.html)

---

### F-08 — USB-writing tools are not verified after download

**Files and lines:**

- `docs/guides/bootable-usb.md:132`
- `docs/guides/bootable-usb.md:158`
- `docs/guides/bootable-usb.md:168`
- `docs/fedora/02-download.md:130-134` (reviewed revision: lines 123-124)

**Severity:** Low to Medium

**Status:** Partially fixed. Fedora Media Writer verification was added; Rufus
and Ventoy verification remains open.

**Instructions:** Download Rufus, Ventoy, or Fedora Media Writer from the
official website and run it.

**Why it is unsafe:** These tools run on the host and write raw disk images. A
tampered download could destroy data or execute unwanted code with the user's
privileges. The ISO checksum is covered, but the tool binary itself is not.

**Recommended correction:**

- Tell users to verify the publisher's signature or checksum before running the
  tool:
  - Rufus: verify the Windows Authenticode signature and the official checksum.
  - Ventoy: compare the downloaded package with the official SHA-256 value.
  - Fedora Media Writer: verify the published checksum if the download page
    provides one.
- Link to the official verification instructions for each tool.
- Do not recommend running tools from unofficial mirrors.

**Official reference:** [Rufus](https://rufus.ie/) · [Ventoy downloads](https://www.ventoy.net/en/download.html) · [Fedora Workstation download](https://fedoraproject.org/workstation/download/)

---

### F-09 — Repository disable command can stop security updates

**File:** `docs/fedora/06-troubleshooting.md:193-207` (reviewed revision: line 193)

**Severity:** Low

**Status:** Fixed in the working tree with an explicit warning and verification
step.

**Instruction:**

```bash
sudo dnf config-manager setopt REPOSITORY-ID.enabled=0
```

**Why it is unsafe:** The placeholder does not say which repositories are safe to
disable. A beginner could substitute `fedora`, `updates`, or `updates-testing` and
stop receiving Fedora packages and security updates. The original text only said
to replace the placeholder with an ID from `dnf repolist`.

**Recommended correction:**

- State explicitly that `fedora`, `updates`, and `updates-testing` must not be
  disabled.
- Limit the instruction to broken or unwanted third-party repositories.
- Show `dnf repolist` before and after the change.
- Show how to re-enable the repository and run `sudo dnf upgrade --refresh`.

**Official reference:** [Fedora DNF documentation](https://docs.fedoraproject.org/en-US/quick-docs/dnf/)

---

## Potential supply-chain items requiring validation

These are not confirmed vulnerabilities. They require a documented threat model
or live verification before severity can be assigned.

### PV-01 — Fedora OpenPGP key is fetched from the same HTTPS origin

**File:** `docs/fedora/02-download.md:105-116` (reviewed revision: lines 104-105)

**Status:** Addressed in the working tree with a fingerprint check. Manual
fingerprint verification remains.

**Unresolved fact:** The keyring is downloaded from `fedoraproject.org` and used
immediately. The page does not tell users to compare the key fingerprint with an
out-of-band source.

**Why it matters:** If the website or its TLS trust chain is compromised, an
attacker could replace both the keyring and the checksum file. HTTPS alone does
not provide an independent trust anchor.

**Validation needed:**

- Confirm the current Fedora key fingerprints on an independent official source.
- Add a fingerprint comparison step to the tutorial.
- Consider using the `fedora-gpg-keys` package from the installed system for
  package verification where appropriate.

**Official reference:** [Fedora security and keys](https://fedoraproject.org/security/)

### PV-02 — Firewall rule exposure depends on the live default zone

**File:** `docs/fedora/06-troubleshooting.md:326` (reviewed revision: line 301)

**Status:** Documentation now restricts the example by zone and source. Live
exposure validation remains.

**Unresolved fact:** The actual default zone on Fedora Workstation 45 and whether
port 8080 is already allowed by the `FedoraWorkstation` zone must be tested on a
live system.

**Why it matters:** The same command has different exposure on
`FedoraWorkstation` and `public`. The documentation should state the exposure
explicitly rather than relying on the reader's zone.

**Validation needed:** Run the rule on a disposable Fedora 45 VM in both zones
and inspect IPv4 and IPv6 exposure. Do not test on a production machine.

### PV-03 — Third-party GNOME extensions and Flatpak permissions

**Files:**

- `docs/fedora/05-desktop-setup.md:134`
- `docs/ubuntu/05-desktop-setup.md:72`

**Unresolved fact:** The tutorials recommend GNOME extensions and Extension
Manager from Flathub, but do not define a permission-review process for
extensions or Flatpak applications.

**Why it matters:** GNOME extensions run inside the desktop session and can
access user data. Flatpak applications are sandboxed, but permissions vary by
application.

**Validation needed:**

- Test a small set of recommended extensions and document their permissions.
- Add guidance to review extension source and Flatpak permissions before
  installation.
- Avoid presenting extension installation as a purely cosmetic action.

## Rejected candidates and adequately warned operations

These items were reviewed and are **not** confirmed vulnerabilities.

| Candidate | Result |
|---|---|
| `curl ... | bash` or `wget ... | sh` | Not present. |
| `--nogpgcheck` or `--nosignature` as a workaround | Only appears as an explicit warning **not** to use it. |
| `rm -rf /` | Appears only as a warning never to run it. |
| `mkfs`, `dd`, `wipefs`, `parted` commands | Not present as executable instructions; only mentioned in warnings. |
| `setenforce 0`, `systemctl disable firewalld`, Secure Boot disable | Not instructed. The guides say not to disable these as a first step. |
| Hardcoded credentials, tokens, private keys | None found in the current tree or Git history. |
| GitHub Actions permissions | No workflows exist. |
| `sudo chown` | Present with a warning to use it only on owned files or official instructions. |
| `rsync --delete` | Present with a dry-run warning and an option to omit `--delete`. |
| `dnf swap --allowerasing` | Present with a clear transaction-review warning. |
| `fwupdmgr update` | Present with a power-loss warning. |
| `rm -r` practice paths | Scoped to `~/linux-practice` or `/tmp/restore-test`; warnings are present. |
| `kill -9` | Present with a warning to try `kill PID` first. |

## Recommended fixes, in priority order

### Priority 1 — Fix `docs/fedora/` first (remediation started)

1. Restrict the firewall example by zone and source, and show removal. **Fixed.**
2. Add bootloader backup, UEFI/BIOS path, and live-USB recovery warnings. **Fixed.**
3. Add a preview step for `dnf history undo`. **Fixed.**
4. Warn not to disable Fedora or updates repositories with
   `dnf config-manager`. **Fixed.**
5. Verify the Fedora Media Writer binary. **Partially fixed.**

### Priority 2 — Fix `docs/guides/`

1. Add consistent warnings to all NVIDIA removal sections.
2. Add a dry-run and one-at-a-time guidance for Arch orphan removal.
3. Add checksum/signature verification for Rufus, Ventoy, and Fedora Media
   Writer.
4. Document out-of-band verification of the Fedora OpenPGP key.

### Priority 3 — Fix `docs/ubuntu/`

1. Remove `ufw disable` from the normal uninstall section or make it explicitly
   temporary.
2. Add a configuration and dependency warning before `apt purge`.
3. Add a bootloader backup warning before `update-grub`.

### Priority 4 — Process hardening

1. Add a short “dangerous command” checklist to `CONTRIBUTING.md`.
2. Require a warning, rollback step, and official reference for every destructive
   command.
3. If GitHub Actions are added later, use least privilege
   (`permissions: contents: read`), pin third-party actions by commit SHA, and
   never use `pull_request_target` with untrusted code.
4. Add a Markdown link checker and a secret scanner in CI once the project has a
   workflow.
5. Add a `SECURITY.md` with a reporting contact before public release.

## Remaining manual testing

These items cannot be verified statically:

- [ ] Test firewall rules on a disposable Fedora 45 VM in both
      `FedoraWorkstation` and `public` zones, over IPv4 and IPv6.
- [ ] Test Fedora `grub2-mkconfig` on UEFI and BIOS systems; verify the correct
      path, backup, and recovery.
- [ ] Test Ubuntu `update-grub` and `os-prober` with Windows and without Windows.
- [ ] Test NVIDIA driver removal on Ubuntu, Fedora, and Arch; confirm the desktop
      still starts or falls back to software rendering.
- [ ] Test `pacman -Rns $(pacman -Qtdq)` with optional dependencies installed.
- [ ] Test `dnf history undo` on a disposable transaction and confirm the preview
      and failure behavior.
- [ ] Test `ufw disable` and re-enable with an active SSH session.
- [ ] Verify the current Rufus, Ventoy, and Fedora Media Writer signatures or
      checksums.
- [ ] Verify Fedora key fingerprints against an independent official source.
- [ ] Test GNOME extension and Flatpak permission review on Fedora 45 and Ubuntu
      26.04.
- [ ] Perform a full Fedora 45 installation and first-boot setup on a VM or spare
      machine.

## Evidence summary

- No `.github/` directory.
- No shell, Python, YAML, Docker, Make, or systemd unit files.
- No executable files or symlinks.
- No secret-pattern matches in the current tree or Git history.
- No `http://` links.
- 65 Markdown files reviewed through static inspection and targeted searches.
- No target commands from the tutorials were executed.

## Recommended next folder to fix

**`docs/guides/`**

The Fedora high-impact items are fixed in the working tree. The next highest-risk
shared guide is `docs/guides/graphics-drivers.md` (broad NVIDIA package removal)
followed by `docs/guides/system-maintenance.md` (Arch orphan removal). Fix those
before moving to the Ubuntu firewall and purge warnings.

- [Back to project README](README.md) · [Documentation home](docs/README.md)
