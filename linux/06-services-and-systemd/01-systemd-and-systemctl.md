# Services and systemctl

When you run a command like `ls` or `mkdir`, the program starts, does its job, and immediately exits. But some programs need to run constantly in the background, waiting to respond to requests—like a container engine (Docker), an automation server (Jenkins), or the SSH daemon (`sshd`) that lets you log in.

In Linux, these background programs are called **daemons** or **services**.

## What is systemd?

When a Linux server boots up, the kernel is loaded into memory. The kernel's very first job is to start exactly one program, which is given the Process ID of `1` (PID 1). 

In modern Linux distributions (like Ubuntu, CentOS, RHEL, and Debian), this first program is almost always **`systemd`**. 

`systemd` is the "mother of all processes". Its job is to manage the entire system: it brings up the network, mounts the file systems, and starts all the other background services (like Docker and Jenkins) in the correct order.

## What is systemctl?

If `systemd` is the manager behind the scenes, **`systemctl`** is the remote control you use to talk to it. 

You use `systemctl` to check the status of services, start them, stop them, or configure them to start automatically when the server reboots.

### Essential `systemctl` Commands

*Note: Managing services almost always requires `sudo` privileges.*

**1. Checking Status**
To see if a service is running, crashed, or stopped:
```bash
sudo systemctl status docker
```
*(Tip: This will also show you the most recent logs for that service).*

**2. Starting and Stopping**
```bash
sudo systemctl start docker   # Starts the service right now
sudo systemctl stop docker    # Stops the service right now
```

**3. Restarting and Reloading**
```bash
sudo systemctl restart docker # Stops and then starts the service (causes brief downtime)
sudo systemctl reload docker  # Tells the service to re-read its config file without stopping (no downtime)
```

**4. Enabling and Disabling (Boot Behavior)**
Starting a service doesn't mean it will survive a reboot. To make a service start automatically every time the server turns on, you must **enable** it:
```bash
sudo systemctl enable docker  # Will start automatically on boot
sudo systemctl disable docker # Will NOT start automatically on boot
```

## How Does systemd Know What to Do? (Unit Files)

How does `systemd` know exactly *how* to start Docker or Jenkins? It reads a configuration file called a **Unit File** (usually ending in `.service`).

These files are typically located in `/etc/systemd/system/` or `/lib/systemd/system/`.

A practical unit file for **Apache Tomcat** (`tomcat.service`) looks like this:
```ini
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking
User=tomcat
Group=tomcat

Environment=JAVA_HOME=/usr/lib/jvm/jre
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```
This file tells `systemd`:
- **When to start**: Wait until the network is up (`After=network.target`).
- **Who to run as**: Execute the process as the `tomcat` user and group for security (`User=tomcat`, `Group=tomcat`).
- **What environment to use**: Sets crucial environment variables like `JAVA_HOME` and `CATALINA_HOME` before starting.
- **How to start/stop**: The exact shell scripts to execute for starting and stopping Tomcat (`ExecStart`, `ExecStop`).
- **Recovery**: If the service crashes, always try to restart it after a 10-second delay (`Restart=always`, `RestartSec=10`).
- **Enablement**: Ties this service to the standard system boot process (`WantedBy=multi-user.target`).

### What are `.target` files? (Understanding `After` and `WantedBy`)

You'll notice `network.target` and `multi-user.target` in the file. In `systemd`, a **target** is simply a logical grouping of services, acting like a milestone during the boot process. 
- **`After=network.target`**: Tomcat needs an IP address to listen on. This tells systemd: *"Don't try to start Tomcat until the networking stack is fully initialized."*
- **`WantedBy=multi-user.target`**: This defines what happens when you run `systemctl enable tomcat`. `multi-user.target` is the standard milestone where a server is fully booted, without a graphical UI, ready for multiple users to log in (this replaced "runlevel 3" in older Linux versions). By saying `WantedBy=multi-user.target`, you are telling systemd: *"When the server reaches its normal operational state, it should pull Tomcat up with it."*

---

### 💡 Key Takeaways
- **Services/Daemons** are programs that run continuously in the background.
- **`systemd`** is the core initialization system (PID 1) that manages all services.
- **`systemctl`** is the command you use to control `systemd`.
- **`start`/`stop`** affect the service right now.
- **`enable`/`disable`** determine if the service starts automatically on boot.
- **Unit Files** (`.service`) contain the blueprints telling `systemd` how to manage a specific application.
