# Contributing to Linux Setup Tutorials

Thanks for helping improve these tutorials! This project is for beginners, so
clarity matters more than completeness.

## Ways to contribute

- Improve existing tutorials or fix errors
- Write a missing tutorial (look for `> 🚧 **Stub page.**`)
- Report broken links
- Translate guides

## Before you start

- Open an issue for large changes so we can agree on the approach.
- Keep each page focused on one topic.
- Test every command you document, if possible.

## Writing style

- Write for someone installing Linux for the first time.
- Use short sentences and numbered steps for procedures.
- Explain *why* a step matters, not just *what* to type.
- Put commands in fenced code blocks:

  ```bash
  sudo apt update
  ```

- Say whether a command needs `sudo` and warn before anything destructive
  (partitioning, formatting, deleting files).
- Link to official documentation instead of copying it.
- Use relative links between pages, for example `../guides/bootable-usb.md`.

## Page structure

Every distribution follows the same learning path:

1. `01-introduction.md`
2. `02-download.md`
3. `03-installation.md`
4. `04-initial-setup.md`
5. `05-desktop-setup.md`
6. `06-troubleshooting.md`

When adding a new distribution:

1. Create `docs/<distribution-name>/` (lowercase, hyphens instead of spaces).
2. Add a `README.md` that lists the learning path — copy an existing one as a template.
3. Add the six step pages.
4. Add the distribution to the table in the project `README.md` and `docs/README.md`.

When adding a general guide:

1. Create `docs/guides/<topic>.md`.
2. Link it from `docs/guides/README.md`, the project `README.md`, and the
   distribution overview pages.

## Replacing a stub

Stub pages contain this notice:

```markdown
> 🚧 **Stub page.** This page is a placeholder and will be written soon.
```

Write the content, then remove the notice. Keep the existing navigation links at
the top of the page.

## Safety

Never suggest disabling security features (Secure Boot, firewalls, SELinux) as a
first step. If a workaround is risky, label it clearly and offer a safer
alternative.

## Submitting changes

1. Fork the repository and create a branch with a descriptive name.
2. Make your changes.
3. Check that links and code blocks render correctly.
4. Open a pull request describing what you changed and why.

## License

License to be selected before public release. By contributing, you agree that
your work will be published under the project license once chosen.
