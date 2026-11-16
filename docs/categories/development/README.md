# Development Setup on Linux

This guide is for beginners who want to use Linux for coding, learning programming, or building software. It explains what to install and how to keep your projects organized.

> **Documentation only:** This page explains the steps and links to official resources. It does not include installation scripts.

## Is this setup right for you?

Choose this setup if you want to:

- Write code for websites, desktop apps, or scripts.
- Learn a programming language such as Python, JavaScript, or Rust.
- Use Git to track changes in your projects.
- Run databases or containers for development.
- Prepare for a job, internship, or open-source contribution.

## Before you start

- Back up important files. See [Linux system maintenance](../../guides/system-maintenance.md).
- Install Linux. See the [distribution list](../../README.md#linux-installation).
- Connect to the internet so you can download tools and updates.
- Decide what you want to build first: a website, a script, a mobile app, or something else.
- Create a folder for your projects, such as `Projects` in your home folder.

## Recommended distributions

Most Linux distributions are good for development. These are beginner-friendly choices:

| Distribution | Why it may suit you |
|---|---|
| [Ubuntu](../../ubuntu/README.md) | Large community and broad support for development tools. |
| [Fedora](../../fedora/README.md) | Modern tools and a focus on new technology. |
| [Linux Mint](../../linux-mint/README.md) | Easy desktop and a familiar layout for Windows users. |
| [openSUSE](../../opensuse/README.md) | Strong system tools and good support for developers. |

## Setup steps

1. **Install Linux.** Choose a distribution from the [distribution list](../../README.md#linux-installation) and follow its installation guide.
2. **Update your system.** Install updates from your software center or package manager. See [Package managers](../../guides/package-managers.md).
3. **Install a code editor.** Good choices include [Visual Studio Code](https://code.visualstudio.com/docs/setup/linux), [VSCodium](https://vscodium.com/), or [Neovim](https://neovim.io/).
4. **Install Git.** Git tracks changes in your code. Check that it is installed by opening a terminal and running `git --version`.
5. **Set up Git.** Add your name and email so your commits have the right author:

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "you@example.com"
   ```

6. **Install a programming language.** Start with one language. Common choices are [Python](https://www.python.org/downloads/), [Node.js](https://nodejs.org/en/download), [Rust](https://www.rust-lang.org/), and [Go](https://go.dev/).
7. **Learn the terminal basics.** You will use the terminal for many development tasks. See [Basic Linux commands](../../guides/basic-commands.md).
8. **Set up containers if needed.** [Docker](https://docs.docker.com/engine/install/) and [Podman](https://podman.io/) help you run software in isolated environments. This step is optional for beginners.
9. **Back up your code.** Use Git with a remote repository, and keep copies of important files. See [Linux system maintenance](../../guides/system-maintenance.md).

## Helpful tools

| Tool | What it does | Official page |
|---|---|---|
| Visual Studio Code | Popular code editor with many extensions. | [code.visualstudio.com](https://code.visualstudio.com/docs/setup/linux) |
| VSCodium | Free build of VS Code without Microsoft telemetry. | [vscodium.com](https://vscodium.com/) |
| Neovim | Fast terminal-based text editor. | [neovim.io](https://neovim.io/) |
| Git | Version control for tracking code changes. | [git-scm.com](https://git-scm.com/download/linux) |
| Python | Beginner-friendly programming language. | [python.org](https://www.python.org/downloads/) |
| Node.js | JavaScript runtime for web and server projects. | [nodejs.org](https://nodejs.org/en/download) |
| Docker | Runs applications in containers. | [docs.docker.com](https://docs.docker.com/engine/install/) |
| Podman | Container tool that does not need a background service. | [podman.io](https://podman.io/) |

Install tools from official sources or your distribution's software center. Avoid random scripts from the internet.

## Beginner tips

- Keep each project in its own folder.
- Learn a few Git commands first: `git status`, `git add`, and `git commit`.
- Read error messages slowly. They often tell you what is wrong.
- Use official documentation for the language or tool you are learning.
- Do not run commands you do not understand.
- Keep your system and development tools updated.

## Related guides

- [Creating bootable USB drives](../../guides/bootable-usb.md)
- [Dual-booting Linux and Windows](../../guides/dual-boot.md)
- [Linux desktop environments](../../guides/desktop-environments.md)
- [Package managers](../../guides/package-managers.md)
- [Basic Linux commands](../../guides/basic-commands.md)
- [Linux system maintenance](../../guides/system-maintenance.md)

## Other setup categories

- [Gaming setup](../gaming/README.md)
- [Work setup](../work/README.md)
- [School setup](../school/README.md)
- [Personal setup](../personal/README.md)

[Back to setup categories](../README.md) · [Back to project README](../../../README.md)
