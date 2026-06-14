# SUDO (Super User Does)

In Linux, there is a special, all-powerful user named `root`. The `root` user has absolute authority over the system: it can read any file, stop any database, and delete the entire hard drive without a second warning.

Because of this immense power, it is a massive security risk to log in directly as `root` or to share the `root` password among a team. This is where `sudo` comes in.

## What is `sudo`?

`sudo` stands for **Super User Does**. It allows a normal user to temporarily elevate their privileges to execute a specific command as `root` (or as another user).

### The Principle of Least Privilege
`sudo` enforces the **Principle of Least Privilege**. You log in as a normal user with limited rights. 95% of the time, that's all you need. For the 5% of the time you need administrative power (like installing a package or restarting a service), you use `sudo` for just that one command.

### The Auditing Benefit
Beyond security, `sudo` provides **accountability**. If three developers share the `root` password, and one accidentally deletes the database, the system logs just show that "`root` deleted the database". You don't know who did it. 

When developers use their own accounts with `sudo`, the system logs exactly *who* ran the elevated command (usually in `/var/log/auth.log` or `/var/log/secure`).

## Using `sudo` vs. `su`

You will often see two different commands used to gain root access: `sudo` and `su`.

- **`sudo <command>`**: Runs a *single command* as root. You are prompted for **your own password**.
- **`su -`** (Switch User): Completely switches your terminal session to the `root` user. You are prompted for the **root password**.
- **`sudo su -`**: A common trick. It uses `sudo` (requiring *your* password) to run the `su -` command, effectively dropping you into a root shell without needing the actual root password.

## The Sudoers File (`/etc/sudoers`)

How does the system know *which* users are allowed to use `sudo`? This is configured in the **sudoers file**, located at `/etc/sudoers`.

### The Golden Rule: Safely Editing with `visudo`
You should **never** edit `/etc/sudoers` directly with a text editor like `vim` or `nano`. If you make a syntax error and save the file, you could break the `sudo` command entirely, locking everyone (including yourself) out of administrative access.

Instead, always use:
```bash
sudo visudo
```
The `visudo` command opens the file, lets you edit it, and critically, **validates the syntax before saving**. If you made a mistake, it warns you and refuses to save the broken configuration.

### Deconstructing the Sudoers Syntax

If you open the sudoers file, you will see rules granting access. Here is the anatomy of a typical `sudoers` rule:

```text
ubuntu  ALL=(ALL:ALL) NOPASSWD: ALL
```

Let's break down what each part means:
1. **`ubuntu`** (Who): The user this rule applies to. *(Note: If it starts with a `%` symbol like `%wheel` or `%admin`, it applies to a **group** instead of a single user).*
2. **`ALL=`** (Where): Specifies which hosts the user can run commands on. Usually, it's just `ALL`.
3. **`(ALL:ALL)`** (As Whom): Means the user can run commands as any user and any group. 
4. **`NOPASSWD:`** (Modifiers): This specific modifier means the user won't be prompted for their password when using `sudo`. This is highly common in automated cloud environments so scripts don't get stuck waiting for a password prompt.
5. **`ALL`** (What): The specific commands the user is allowed to run. `ALL` means everything. If it was `/bin/systemctl restart nginx`, the user could *only* restart nginx and nothing else.

---

### 💡 Key Takeaways
- `sudo` enforces the **Principle of Least Privilege** and provides accountability via system logging.
- `sudo` runs a single command; `su -` switches the entire shell session.
- **Never edit `/etc/sudoers` directly.** Always use `sudo visudo` to prevent syntax errors that lock you out.
- Sudo rules define *Who*, *Where*, *As Whom*, and *What Commands* can be run. The `%` prefix denotes a group.
