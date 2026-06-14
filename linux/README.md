# Linux Training Materials

<div align="center">
  <h3>🐧 From Zero to Confident on the Command Line</h3>
  <p><em>Supplementary notes for concepts not covered in hands-on sessions</em></p>
</div>

## 📚 Course Overview

These notes cover the **conceptual foundations** of Linux — the "why" and "what" behind the commands and workflows you practice in class. They're designed to complement your hands-on training, not replace it.

## 🎯 Learning Path

### 01 - Introduction
Foundational concepts — what Linux is, why it matters, and the landscape of distributions.
- **[What is Linux?](./01-introduction/01-what-is-linux.md)** — Kernel vs OS, history, and core architecture
- **[Why Linux for Deployment](./01-introduction/02-why-linux-for-deployment.md)** — Cost, security, performance, and why the industry runs on Linux
- **[Linux Distributions](./01-introduction/03-linux-distributions.md)** — Distro families, package managers, and choosing the right one

### 02 - Shell and Navigation
Getting comfortable with the terminal and navigating the file system.
- **[Intro to Shell / Terminal](./02-shell-and-navigation/01-intro-to-shell.md)** — What the shell is, shell vs terminal, the prompt, and essential shortcuts
- **[Home Directory](./02-shell-and-navigation/02-home-directory.md)** — Your personal space, the `~` shortcut, and hidden config files
- **[Commands and Arguments](./02-shell-and-navigation/03-commands-and-arguments.md)** — Anatomy of a command, options, flags, and getting help
- **[Relative Path vs Absolute Path](./02-shell-and-navigation/04-relative-vs-absolute-path.md)** — Two ways to describe a location, when to use which
- **[Wildcards and File Globbing](./02-shell-and-navigation/05-wildcards-and-globbing.md)** — Using `*`, `?`, and `[]` to match multiple files

### 03 - I/O and Data Streams
Understanding how data flows through Linux commands.
- **[Data Streams — stdin, stdout, and stderr](./03-io-and-data-streams/01-data-streams.md)** — The three streams every command uses, and why they're separate

### 04 - Users and Permissions
Managing who has access to the system and what they can do.
- **[SUDO (Super User Does)](./04-users-and-permissions/01-sudo-and-permissions.md)** — Running commands with root privileges and understanding `/etc/sudoers`

### 05 - Networking and SSH
Connecting to remote systems securely.
- **[SSH (Secure Shell)](./05-networking-and-ssh/01-ssh-protocol.md)** — The protocol, common ports, and setting up passwordless key-based access

### 06 - Services and systemd
Managing background processes and the system initialization.
- **[Services and systemctl](./06-services-and-systemd/01-systemd-and-systemctl.md)** — Understanding `systemd`, managing services with `systemctl`, and how unit files work

---

## 🚀 Quick Start

1. **Brand new to Linux?** Start with [What is Linux?](./01-introduction/01-what-is-linux.md)
2. **Know the basics, want context?** Jump to [Why Linux for Deployment](./01-introduction/02-why-linux-for-deployment.md)
3. **Confused about Ubuntu vs CentOS vs Alpine?** Read [Linux Distributions](./01-introduction/03-linux-distributions.md)

## 📋 Course Structure

```
linux/
├── 01-introduction/               # What, why, and the landscape
├── 02-shell-and-navigation/       # Terminal, paths, commands, wildcards
├── 03-io-and-data-streams/        # stdin, stdout, stderr, pipes
├── 04-users-and-permissions/      # sudo, root, sudoers
├── 05-networking-and-ssh/         # SSH, ports, keys
├── 06-services-and-systemd/       # systemd, systemctl, daemons
├── images/                        # All screenshots and diagrams
└── README.md                      # This file
```

## 💡 How to Use These Notes

- These notes **supplement** the live training — they cover concepts where written reference material helps.
- Each topic has **Key Takeaways** at the bottom for quick revision.
- Navigation links at the bottom of each page let you move through topics sequentially.

---
*Happy Learning! 🚀*
