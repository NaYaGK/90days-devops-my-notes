# Day 1 - AWS EC2 & Linux Basics

## Agenda

- Create an EC2 Instance (Amazon Linux 2023) in AWS.
- Configure SSH using a `config` file for easy access.
- Learn basic Linux system information commands.
- Understand the default EC2 user and SSH key usage.

---

# 1. Create an EC2 Instance

Create an **Amazon Linux 2023** EC2 instance in AWS.

After launching the instance:

- Download the `.pem` SSH key.
- Note the **Public IP Address**.
- Ensure port **22 (SSH)** is allowed in the Security Group.

---

# 2. SSH Configuration (`~/.ssh/config`)

After creating the EC2 instance, open Terminal on your local machine.

```bash
cd ~/.ssh
```

This enters the `.ssh` directory.

> **Note**
>
> Although you can create a `config` file anywhere (Downloads, Documents, etc.), the recommended location is:
>
> ```
> ~/.ssh/config
> ```

You can create the file using any editor:

```bash
touch config
```

or

```bash
nano config
```

or

```bash
vim config
```

---

## Sample SSH Config

```text
Host devops-ec2
    HostName 65.2.190.72
    User ec2-user
    IdentityFile ~/Downloads/formac.pem
    StrictHostKeyChecking no
```

---

## Explanation

### Host

```text
Host devops-ec2
```

This is an alias (nickname).

You can choose any name.

Examples:

- devops-ec2
- myserver
- aws-server
- production

Instead of remembering the IP address, you'll simply use:

```bash
ssh devops-ec2
```

---

### HostName

```text
HostName 65.2.190.72
```

This is the EC2 instance's **Public IP Address**.

If the public IP changes, simply update this line.

---

### User

```text
User ec2-user
```

Default usernames for common AMIs:

| Operating System | Default User |
|------------------|--------------|
| Amazon Linux | `ec2-user` |
| Ubuntu | `ubuntu` |
| Debian | `debian` |
| CentOS | `centos` |

---

### IdentityFile

```text
IdentityFile ~/Downloads/formac.pem
```

Location of your SSH private key (`.pem`).

Set proper permissions:

```bash
chmod 400 ~/Downloads/formac.pem
```

SSH requires private keys to have secure permissions.

---

### StrictHostKeyChecking

```text
StrictHostKeyChecking no
```

This prevents SSH from asking:

> "Are you sure you want to continue connecting?"

Useful for:

- Practice
- Labs
- Testing

For production environments, keep host key checking enabled for better security.

---

# Connecting to EC2

After saving the config:

```bash
ssh devops-ec2
```

Instead of:

```bash
ssh -i ~/Downloads/formac.pem ec2-user@65.2.190.72
```

---

## If Config File Is Not Inside `~/.ssh`

Example:

```
~/Downloads/config
```

Connect using:

```bash
ssh -F ~/Downloads/config devops-ec2
```

`-F` tells SSH to use an alternate configuration file.

**Recommendation:**

Always keep the config file inside:

```text
~/.ssh/config
```

---

# 3. System Information Commands

---

## Current User

```bash
whoami
```

Example:

```text
ec2-user
```

Shows the currently logged-in user.

---

## Kernel Release

```bash
uname -r
```

Example:

```text
6.18.33-63.124.amzn2023.x86_64
```

Explanation:

- `uname` = Unix Name
- `-r` = Kernel Release

Displays the Linux kernel version.

---

## Server Uptime

```bash
uptime
```

Example:

```text
05:47:49 up 40 min, 2 users, load average: 0.00, 0.00, 0.00
```

Shows:

- Current time
- How long the server has been running
- Logged-in users
- System load average

---

## Memory Information

```bash
free -h
```

Example:

```text
               total        used        free      shared  buff/cache   available
Mem:           912Mi       158Mi       499Mi       255Mi       623Mi
Swap:             0B          0B          0B
```

Options:

- `-h` = Human-readable format

Shows:

- Total Memory
- Used Memory
- Free Memory
- Available Memory
- Swap Memory

---

## Disk Space

```bash
df -h
```

Example:

```text
Filesystem      Size Used Avail Use%
/dev/nvme0n1p1   8G  1.8G  6.2G  22%
```

Displays:

- Filesystem
- Total Disk
- Used Space
- Available Space
- Usage Percentage

---

## Running Processes

```bash
ps aux | head -10
```

Example:

```text
USER PID %CPU %MEM COMMAND
root   1 0.0 1.8 /usr/lib/systemd/systemd
...
```

Explanation:

- `ps` = Process Status
- `a` = All users
- `u` = User-oriented output
- `x` = Include background processes
- `head -10` = Display the first 10 lines

Shows currently running processes.

---

# Important Observations

- SSH works using an alias instead of typing the full command.
- Amazon Linux uses `ec2-user` as the default login user.
- Memory on a `t2.micro` instance is approximately **1 GB**.
- Available disk space is around **6–7 GB** after the OS installation.
- Amazon Linux 2023 uses the Linux 6.x kernel series.

---

# Commands Summary

| Purpose | Command |
|----------|---------|
| Go to SSH directory | `cd ~/.ssh` |
| Current user | `whoami` |
| Kernel version | `uname -r` |
| System uptime | `uptime` |
| Memory usage | `free -h` |
| Disk usage | `df -h` |
| Running processes | `ps aux \| head -10` |
| Connect using config | `ssh devops-ec2` |
| Connect using alternate config | `ssh -F ~/Downloads/config devops-ec2` |
| Set key permissions | `chmod 400 ~/Downloads/formac.pem` |

---

# What I Learned Today

- ✅ Created an Amazon Linux EC2 instance.
- ✅ Connected to EC2 using SSH.
- ✅ Learned how SSH configuration files simplify connections.
- ✅ Understood why SSH key permissions (`chmod 400`) are required.
- ✅ Learned the purpose of `whoami`, `uname`, `uptime`, `free`, `df`, and `ps`.
- ✅ Understood that `ec2-user` is the default non-root user on Amazon Linux.
- ✅ Learned how to monitor server memory, disk, uptime, and running processes.

---
**End of Day 1 Notes**