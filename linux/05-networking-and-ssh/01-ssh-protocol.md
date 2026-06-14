# SSH (Secure Shell)

While you already know the basic commands to connect to a server (`ssh -i key.pem user@ip`), understanding *how* and *why* SSH works will make troubleshooting connection issues much easier.

When servers are hosted in data centers or the cloud (like AWS, Azure, or GCP), you need a way to log in over the internet. **SSH** (Secure Shell) is the industry-standard network protocol that provides a secure, encrypted channel to access and manage remote servers.

## Why SSH? (The Death of Telnet)
Before SSH, administrators used protocols like **Telnet** or **rlogin** to connect to servers. The problem? **Everything was sent in plain text.** If you typed your password over a Telnet connection, anyone monitoring the network traffic between your laptop and the server could read it. 

SSH solved this by encrypting all traffic. Even if someone intercepts your connection, all they see is cryptographic noise.

## Understanding Ports

To connect to a server, you need to know which "door" to knock on. These doors are called **ports**. 
- A server has one IP address (the building).
- A server has 65,535 ports (the doors).

Different services listen on different default ports:
- **SSH**: 22 (This is where the SSH daemon, `sshd`, listens for incoming connections)
- **HTTP** (Unencrypted Web): 80
- **HTTPS** (Encrypted Web): 443
- **Jenkins / Tomcat**: 8080
- **Kubernetes API Server**: 6443

*Note: For security reasons, system administrators sometimes change the default SSH port from 22 to a random high number (like 2222) to avoid automated bot attacks.*

## How SSH Key Authentication Works Under the Hood

Typing passwords every time is tedious and vulnerable to brute-force attacks. **SSH Key Pairs** solve this using *Asymmetric Cryptography*.

1. **Public Key (`id_rsa.pub`)**: You can share this with the world. Think of it as a specialized padlock. You put this padlock on any server you want to access.
2. **Private Key (`id_rsa` or `.pem`)**: This is the only key that can open the padlock. It never leaves your computer.

### The Handshake:
When you run `ssh -i private-key ubuntu@172.31.36.45`:
1. Your computer knocks on port 22 of the server.
2. The server says, "I see you want to log in as `ubuntu`. I have a padlock (public key) for that user. Prove you have the key."
3. The server encrypts a random mathematical puzzle using the **public key** and sends it to you.
4. Your computer uses your **private key** to decrypt the puzzle, solves it, and sends the answer back.
5. The server verifies the answer. If it's correct, you are granted access. **Your private key is never sent over the network.**

## The `~/.ssh` Directory Structure

When working with SSH, you'll frequently interact with the hidden `.ssh` directory in your user's home folder. Here are the critical files inside it:

- **`authorized_keys`** (On the Server): A list of all the public keys (padlocks) that are allowed to log in as this user. To grant someone access, you append their public key text to this file.
- **`known_hosts`** (On your Local Machine): Every time you connect to a *new* server, SSH asks "Are you sure you want to continue connecting?". If you say yes, it saves the server's unique fingerprint in this file. This prevents "Man-in-the-Middle" attacks where a hacker tries to impersonate the server.
- **`id_rsa` / `id_rsa.pub`**: Your default private and public keys.

### The "Permissions Are Too Open" Error
SSH is extremely strict about file permissions. If your private key can be read by other users on your local machine, SSH will refuse to use it and throw a `WARNING: UNPROTECTED PRIVATE KEY FILE!` error.

To fix this, you must restrict the permissions so *only you* can read it:
```bash
chmod 400 my-aws-key.pem  # Read-only for the owner, no access for anyone else
```

## Pro-Tip: The SSH Config File

Tired of typing long SSH commands like `ssh -i ~/.ssh/my-aws-key.pem ubuntu@172.31.36.45`?

You can create a file on your local machine at `~/.ssh/config` to save connection profiles:

```text
Host prod-db
    HostName 172.31.36.45
    User ubuntu
    IdentityFile ~/.ssh/my-aws-key.pem
```

With this configured, you can simply type:
```bash
ssh prod-db
```
The SSH client automatically reads the config file, fetches the IP, username, and key, and logs you in!

---

### 💡 Key Takeaways
- **SSH encrypts all traffic**, replacing insecure protocols like Telnet.
- The SSH service runs on **Port 22** by default.
- **Asymmetric keys** provide passwordless access: Public keys go in the server's `~/.ssh/authorized_keys`, while the Private key stays local.
- Private keys must have strict permissions (e.g., `chmod 400`), or SSH will reject them.
- Use the `~/.ssh/config` file to create shortcuts for servers you connect to frequently.
