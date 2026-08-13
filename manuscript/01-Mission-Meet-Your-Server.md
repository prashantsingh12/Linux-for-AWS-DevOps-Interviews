# Chapter 1

# 🖥️ Mission 1 – Meet Your Server

---

## 🎯 Mission Brief

Congratulations!

Today is your first day as a Cloud Engineer.

Your manager has provisioned an Amazon EC2 instance and shared the SSH credentials. The application running on this server is experiencing intermittent issues, and your team needs your help.

As you prepare to investigate the system, your manager gives you one instruction.

> **"Don't change anything. First, understand the server."**

At first, this advice may seem overly cautious.

Why not restart the service?

Why not install the missing package?

Why not begin troubleshooting immediately?

Because experienced engineers know that many production incidents are caused not by lack of knowledge, but by making assumptions.

Before touching an unfamiliar server, they collect information.

They answer questions.

Only then do they make changes.

Throughout this mission, you'll develop the same habit.

By the end of this chapter, you'll be able to connect to an unfamiliar Linux server and quickly answer the questions every experienced engineer asks before taking action.

Remember the philosophy that will guide this entire book.

> **Observe. Think. Verify. Act.**

---

# 🎯 Learning Objectives

By the end of this mission, you'll be able to:

- Identify the Linux distribution running on a server.
- Identify the Linux kernel version and system architecture.
- Determine the currently logged-in user.
- Verify the hostname.
- Determine your current working directory.
- Inspect files and directories.
- Check available disk space.
- Develop the habit of investigating before acting.

---

# 📍 Scene 1 – Which Operating System Is Running?

---

## 💭 The Situation

You have successfully connected to an Amazon EC2 instance.

The terminal displays:

```
[ec2-user@ip-10-0-1-25 ~]$
```

The cursor is blinking.

The server is waiting.

Many beginners immediately begin installing software or restarting services.

You don't.

Instead, you ask yourself:

> **"What operating system am I working with?"**

The answer determines:

- Which package manager is available.
- Which repositories are configured.
- Which commands are supported.
- Where important configuration files are stored.

Never assume.

Always verify.

---

## ❓ Question

**Which operating system is running on this server?**

---

## 🔧 Tool 1 — `cat`

### Purpose

Display the contents of a text file.

### Syntax

```
cat <filename>
```

### AWS Example

```
cat /etc/os-release
```

Amazon Linux

```
NAME="Amazon Linux"
VERSION="2023"
ID="amzn"
```

Ubuntu

```
NAME="Ubuntu"
VERSION="24.04 LTS"
ID=ubuntu
```

RHEL

```
NAME="Red Hat Enterprise Linux"
VERSION="9.4"
ID="rhel"
```

---

## 🔧 Tool 2 — `uname`

Knowing the Linux distribution is only part of the picture.

Engineers often need to know:

- Which Linux kernel is running?
- Which CPU architecture is being used?

The `uname` command answers these questions.

### Syntax

```
uname -a
```

Example

```
Linux ip-10-0-1-25 6.1.140-154.222.amzn2023.x86_64 x86_64 GNU/Linux
```

This output includes:

- Kernel name
- Hostname
- Kernel version
- CPU architecture

---

## 💡 Why Does This Matter?

Imagine your manager asks:

> "Please install Nginx."

Without checking the operating system, you immediately execute:

```
sudo apt install nginx
```

The command fails.

Why?

Because the server is running Amazon Linux.

The correct command is:

```
sudo dnf install nginx
```

Likewise, suppose you're asked to install a software package that is available only for **x86_64** systems.

Before downloading the wrong binary, you verify:

```
uname -a
```

One minute of verification can prevent hours of troubleshooting.

---

## 📘 Package Managers

|Distribution|Package Manager|
|---|---|
|Ubuntu|apt|
|Amazon Linux 2023|dnf|
|Amazon Linux 2|yum|
|RHEL 8+|dnf _(yum is still available as a compatibility alias)_|
|RHEL 7|yum|

---

## 🚀 Tool Evolution

Identify the operating system

```
cat /etc/os-release
```

Display the Linux kernel

```
uname -r
```

Display the system architecture

```
uname -m
```

Display complete system information

```
uname -a
```

---

## ☁ AWS Tip

On many EC2 instances, the hostname is automatically generated and may look like:

```
ip-10-0-1-25
```

This is perfectly normal.

Later in this book, you'll learn how to identify an EC2 instance using the **Instance Metadata Service (IMDS)**.

For now, simply remember that the hostname isn't always a meaningful server name in AWS environments.

---

## 🎤 Interview Corner

**Question**

You've connected to an unfamiliar EC2 instance.

Before installing any software, what information would you collect?

**Expected Discussion**

A strong answer might be:

> "I'd first identify the Linux distribution using `cat /etc/os-release`, then check the kernel version and architecture with `uname -a`. This helps me understand the environment before making any changes."

Interviewers appreciate candidates who explain **why** they execute a command—not just **which** command they use.

---

## ❌ Common Errors → Root Cause

|Problem|First Thing to Check|
|---|---|
|`apt: command not found`|Verify the Linux distribution.|
|Installed the wrong package|Check `/etc/os-release`.|
|Wrong binary downloaded|Check `uname -m`.|
|Driver incompatibility|Check `uname -r`.|

---

## 🧠 Knowledge Check

**1. Which command displays the Linux distribution?**

- ☐ `hostname`
- ☐ `pwd`
- ☐ `cat /etc/os-release`
- ☐ `whoami`

---

**2. Which command displays the Linux kernel version?**

- ☐ `uname -r`
- ☐ `pwd`
- ☐ `hostname`
- ☐ `ls`

---

## ⭐ Engineering Habit

> **Never install software before identifying the operating system and kernel.**

---

## ✅ Scene Summary

In this scene, you learned how to:

- Identify the Linux distribution.
- Determine the kernel version.
- Check the CPU architecture.
- Select the correct package manager.
- Avoid making assumptions.

---

## 📊 Mission Progress

```
██████░░░░░░░░░░

✔ Operating System

○ Current User

○ Hostname

○ Current Directory

○ Directory Contents

○ Disk Space
```

---

# 📍 Scene 2 – Who Am I on This Server?

---

## 💭 The Situation

You've identified the operating system and confirmed the Linux kernel.

Before investigating the application, you ask another important question:

> **"Which user account am I using?"**

Every command you execute on Linux runs with the permissions of the currently logged-in user.

Those permissions determine:

- Which files you can read.
- Which services you can restart.
- Which software you can install.
- Whether administrative commands will succeed.

Understanding your identity is the first step toward understanding your permissions.

---

## ❓ Question

**Which user account am I currently using?**

---

## 🔧 Tool 1 — `whoami`

### Purpose

Display the username of the currently logged-in user.

### Syntax

```
whoami
```

---

### AWS Example

```
whoami
```

Output on Amazon Linux

```
ec2-user
```

Output on Ubuntu

```
ubuntu
```

Output on RHEL

```
ec2-user
```

> **AWS Note**
> 
> The default login user depends on the Amazon Machine Image (AMI) you launch.

|AMI|Default User|
|---|---|
|Amazon Linux|`ec2-user`|
|Ubuntu|`ubuntu`|
|Red Hat Enterprise Linux|`ec2-user`|
|Debian|`admin` (or configured during setup)|

---

## 🔧 Tool 2 — `id`

Knowing the username is useful.

Knowing the user's identity is even better.

The `id` command displays:

- User ID (UID)
- Primary Group ID (GID)
- Supplementary Groups

### Syntax

```
id
```

Example

```
uid=1000(ec2-user)
gid=1000(ec2-user)
groups=1000(ec2-user),10(wheel),998(docker)
```

This tells you much more than `whoami`.

It explains **what the user is allowed to do.**

---

## 🔧 Tool 3 — `groups`

Sometimes you only need to know which Linux groups your account belongs to.

```
groups
```

Example

```
ec2-user wheel docker
```

Membership in groups often determines access to services and resources.

For example:

- `wheel` → Administrative privileges (on many Linux distributions)
- `docker` → Permission to interact with Docker without `sudo`

---

## 🔧 Tool 4 — `sudo -l`

One of the most overlooked yet useful commands is:

```
sudo -l
```

This command lists the commands the current user is permitted to execute using `sudo`.

Example

```
User ec2-user may run the following commands:
    (ALL) NOPASSWD: ALL
```

If you receive a **"user is not allowed to run sudo"** error, this command quickly tells you whether the issue is related to permissions or configuration.

> **Note:** Some organizations restrict access to `sudo -l`. If you receive a permission error, that's expected in certain environments.

---

## 💡 Why Does This Matter?

Imagine your manager asks you to restart Nginx.

You execute:

```
sudo systemctl restart nginx
```

Linux responds:

```
Sorry, user ec2-user is not allowed to execute '/usr/bin/systemctl restart nginx' as root.
```

Instead of repeatedly trying the command, an experienced engineer investigates.

Questions to ask:

- Who am I?
- Which groups do I belong to?
- What am I allowed to execute using `sudo`?

Those three questions usually reveal the root cause much faster than guessing.

---

## 🚀 Tool Evolution

Display the current user

```
whoami
```

Display user and group IDs

```
id
```

Display only group memberships

```
groups
```

List available sudo permissions

```
sudo -l
```

---

## ☁ AWS Scenario

You've joined a new DevOps team.

A teammate says:

> "Docker is already installed, but `docker ps` returns a permission denied error."

Before reinstalling Docker or changing configuration files, you investigate.

```
whoami
```

```
groups
```

Output

```
ec2-user wheel
```

Notice anything missing?

The user isn't a member of the `docker` group.

The problem isn't Docker.

The problem is permissions.

This systematic approach saves valuable troubleshooting time.

---

## 🎤 Interview Corner

**Question**

You've logged into an EC2 instance, but every command requiring administrative privileges is failing.

How would you troubleshoot the issue?

**Expected Discussion**

A strong candidate might answer:

> "I'd begin by identifying the current user with `whoami`, inspect the user's groups using `id` or `groups`, and then verify which commands the user is allowed to execute with `sudo -l`. That helps determine whether the issue is related to permissions before making any changes."

Notice the focus is on **diagnosis**, not trial and error.

---

## ❌ Common Errors → Root Cause

|Problem|First Thing to Check|
|---|---|
|Permission denied|`whoami`|
|Unable to use Docker|`groups`|
|Sudo command fails|`sudo -l`|
|Access denied to a file|`id` and file permissions|

---

## 🧠 Knowledge Check

### 1. Which command displays your current username?

- ☐ `hostname`
- ☐ `whoami`
- ☐ `pwd`
- ☐ `uname`

---

### 2. Which command displays your UID and group memberships?

- ☐ `groups`
- ☐ `sudo`
- ☐ `id`
- ☐ `cat`

---

### 3. Which command shows what you're allowed to run with `sudo`?

- ☐ `sudo -l`
- ☐ `groups`
- ☐ `whoami`
- ☐ `hostname`

---

## ⭐ Engineering Habit

> **Always understand your permissions before attempting administrative tasks.**

Knowing who you are is often more important than knowing the command you want to execute.

---

## 🛠 Production Tip

When investigating a production issue, don't immediately request root access.

First determine whether your existing permissions are sufficient.

Many incidents are resolved without requiring elevated privileges.

---

## ✅ Scene Summary

By completing this scene, you've learned how to:

- Identify the current user.
- Understand user and group IDs.
- Verify group memberships.
- Inspect `sudo` permissions.
- Troubleshoot common permission-related issues.

---

## 📊 Mission Progress

```
████████░░░░░░░░

✔ Operating System

✔ Current User

○ Hostname

○ Current Directory

○ Directory Contents

○ Disk Space
```



# 📍 Scene 3 – Which Server Am I Connected To?

---

## 💭 The Situation

You've confirmed:

- The operating system.
- The Linux kernel.
- Your user account and permissions.

Just then, your manager sends a message:

> **"Please restart the Nginx service on the production web server."**

You have three SSH sessions open:

- Development
- Testing
- Production

All three terminal windows look almost identical.

Before typing a single command, you pause and ask:

> **"Am I connected to the correct server?"**

Experienced engineers never assume they're on the right machine.

They verify.

---

## ❓ Question

**Which server am I connected to?**

---

## 🔧 Tool 1 — `hostname`

### Purpose

Display the hostname of the current Linux system.

### Syntax

```
hostname
```

### AWS Example

```
hostname
```

Example Output

```
ip-10-0-1-25
```

This is the hostname assigned to the current EC2 instance.

---

## 🔧 Tool 2 — `hostnamectl`

### Purpose

Display detailed information about the system.

### Syntax

```
hostnamectl
```

Example Output

```
 Static hostname: ip-10-0-1-25
 Operating System: Amazon Linux 2023
 Kernel: Linux 6.1.x
 Architecture: x86_64
```

Unlike `hostname`, this command provides additional information including:

- Hostname
- Operating System
- Kernel Version
- Architecture

It gives you a quick health snapshot of the machine.

---

## 💡 Why Does This Matter?

Imagine you're responsible for the following servers.

|Hostname|Purpose|
|---|---|
|web-dev-01|Development Web Server|
|web-test-01|Testing Web Server|
|web-prod-01|Production Web Server|
|db-prod-01|Production Database|

You intend to restart Nginx.

Instead, you're accidentally connected to:

```
db-prod-01
```

Executing the wrong command on the wrong server can interrupt production, affect customers, or even cause data loss.

A simple verification takes less than a second.

Recovering from a production outage can take hours.

---

## 🚀 Tool Evolution

Display the hostname

```
hostname
```

Display detailed system information

```
hostnamectl
```

Display the Fully Qualified Domain Name (FQDN)

```
hostname -f
```

Example

```
web-prod-01.company.internal
```

Display all assigned IP addresses

```
hostname -I
```

Example

```
10.0.1.25 172.31.8.15
```

This is useful when verifying network configuration or troubleshooting connectivity.

---

## ☁ AWS Tip

On Amazon EC2, the hostname is often automatically generated.

For example:

```
ip-10-0-1-25
```

This doesn't necessarily tell you which application or environment the instance belongs to.

When working in AWS, engineers often verify the EC2 instance itself using the **Instance Metadata Service (IMDS)**.

For example:

```
curl http://169.254.169.254/latest/meta-data/instance-id
```

Example Output

```
i-0123456789abcdef0
```

> **Note**
> 
> Modern EC2 instances commonly use **IMDSv2**, which requires a session token before accessing metadata. We'll learn IMDS and instance metadata in detail in a later AWS-focused mission. For now, remember that the hostname alone may not uniquely identify the instance in every environment.

---

## ☁ AWS Scenario

CloudWatch sends an alert.

> **High CPU utilization detected on Production Web Server**

You SSH into an EC2 instance.

Before checking CPU usage, you verify:

```
hostname
```

Output

```
web-test-01
```

You're connected to the testing server—not production.

You reconnect to the correct instance before continuing.

Thirty seconds of verification prevented unnecessary troubleshooting on the wrong machine.

---

## 🎤 Interview Corner

**Question**

You've just connected to an unfamiliar EC2 instance.

How would you confirm that you're working on the intended server?

**Expected Discussion**

> "I'd begin by checking the hostname using `hostname` or `hostnamectl`. In AWS environments, if I need to identify the exact EC2 instance, I can also use the Instance Metadata Service to retrieve the instance ID. This ensures I'm troubleshooting or administering the correct machine."

Notice that this answer demonstrates both Linux fundamentals and AWS awareness.

---

## ❌ Common Errors → Root Cause

|Problem|First Thing to Check|
|---|---|
|Restarted the wrong service|Verify the hostname first.|
|Connected to the wrong environment|Check the hostname or EC2 instance ID.|
|Investigating the wrong server|Verify the hostname before troubleshooting.|
|Confused between multiple SSH sessions|Run `hostname` immediately after login.|

---

## 🧠 Knowledge Check

### 1. Which command displays the hostname?

- ☐ `hostname`
- ☐ `whoami`
- ☐ `pwd`
- ☐ `ls`

---

### 2. Which command provides additional system information, including the operating system and kernel version?

- ☐ `hostnamectl`
- ☐ `hostname`
- ☐ `uname`
- ☐ `cat`

---

### 3. In AWS, why might the hostname alone be insufficient?

- ☐ Because every EC2 instance has the same hostname.
- ☐ Because hostnames are often auto-generated and may not clearly identify the instance or environment.
- ☐ Because `hostname` only works on Ubuntu.
- ☐ Because EC2 instances do not have hostnames.

---

## ⭐ Engineering Habit

> **Verify the identity of the server before making any administrative change.**

A single command can prevent an expensive production mistake.

---

## 🛠 Production Tip

Many experienced engineers customize their shell prompt to display the environment (DEV, TEST, or PROD) in different colors. This provides a constant visual reminder of where they're working and reduces the risk of running commands on the wrong server.

---

## ✅ Scene Summary

By completing this scene, you learned how to:

- Display the hostname of a Linux system.
- View additional system details using `hostnamectl`.
- Understand why EC2 hostnames are often auto-generated.
- Recognize when to use the EC2 Instance Metadata Service.
- Develop the habit of confirming the server's identity before making changes.

---

## 📊 Mission Progress

```
██████████░░░░░░

✔ Operating System

✔ Current User

✔ Hostname

○ Current Directory

○ Directory Contents

○ Disk Space
```


# 📍 Scene 4 – Where Am I?

---

## 💭 The Situation

You've confirmed:

- The operating system.
- The Linux kernel.
- Your user account.
- The server you're connected to.

Your manager now sends another message.

> **"The application configuration is stored on the server. Please review it before we continue."**

You type:

```
cat application.yml
```

Linux responds:

```
cat: application.yml: No such file or directory
```

Has the file been deleted?

Maybe.

Or perhaps you're simply looking in the wrong place.

Before searching for the file, ask yourself:

> **"Where am I?"**

Many Linux errors have nothing to do with the command itself.

They're caused by executing the command from the wrong directory.

---

## ❓ Question

**Which directory am I currently working in?**

---

## 🔧 Tool 1 — `pwd`

### Purpose

Display the absolute path of your current working directory.

### Syntax

```
pwd
```

---

### AWS Example

```
pwd
```

Example Output

Amazon Linux

```
/home/ec2-user
```

Ubuntu

```
/home/ubuntu
```

The output tells you exactly where you are in the Linux filesystem.

---

## 💡 Why Does This Matter?

Linux interprets many commands relative to your current location.

Consider the following command.

```
cat config.yaml
```

Will it succeed?

It depends.

If **config.yaml** exists in your current directory, Linux displays the file.

If not, you'll receive:

```
cat: config.yaml: No such file or directory
```

The file may exist elsewhere.

The problem may simply be your location.

---

## 🚀 Tool Evolution

Display the current directory

```
pwd
```

Return to your home directory

```
cd
```

or

```
cd ~
```

Verify your location

```
pwd
```

Output

```
/home/ec2-user
```

Move to the root directory

```
cd /
```

Verify

```
pwd
```

Output

```
/
```

Return to the previous directory

```
cd -
```

This command is extremely useful when switching between two directories during troubleshooting.

---

## ☁ AWS Scenario

You're following a deployment guide.

Step 5 says:

> **Run the deployment script from the application directory.**

Instead of immediately executing

```
./deploy.sh
```

you verify your location.

```
pwd
```

Output

```
/home/ec2-user
```

The application actually resides in:

```
/opt/webapp
```

You navigate there first.

A ten-second verification prevents a failed deployment.

---

## 🎤 Interview Corner

**Question**

You execute:

```
cat nginx.conf
```

and Linux returns:

```
No such file or directory
```

How would you troubleshoot this?

**Expected Discussion**

> "I'd first verify my current working directory using `pwd`. If I'm not in the expected location, I'll navigate to the appropriate directory or use the file's absolute path before assuming the file is missing."

This answer demonstrates logical troubleshooting instead of guesswork.

---

## ❌ Common Errors → Root Cause

|Problem|First Thing to Check|
|---|---|
|No such file or directory|Verify your current directory with `pwd`.|
|Script won't execute|Confirm you're in the correct directory.|
|Editing the wrong file|Verify the file path before opening it.|
|Relative path not working|Use an absolute path or navigate correctly.|

---

## 🧠 Knowledge Check

### 1. Which command displays your current working directory?

- ☐ `ls`
- ☐ `pwd`
- ☐ `hostname`
- ☐ `whoami`

---

### 2. Which command returns you to your home directory?

- ☐ `cd`
- ☐ `pwd`
- ☐ `ls`
- ☐ `hostname`

---

### 3. Which command returns you to the previous directory?

- ☐ `cd -`
- ☐ `cd ..`
- ☐ `pwd`
- ☐ `cd /`

---

## ⭐ Engineering Habit

> **Always know your location before working with files.**

Most file-related mistakes happen because engineers assume they're in the correct directory.

---

## 🛠 Production Tip

When working on production systems, prefer **absolute paths** for important commands.

For example:

Instead of

```
vi nginx.conf
```

use

```
sudo vi /etc/nginx/nginx.conf
```

Absolute paths reduce ambiguity and make command history much easier to understand.

---

## ✅ Scene Summary

By completing this scene, you learned how to:

- Display your current working directory.
- Navigate between directories.
- Understand the difference between relative and absolute paths.
- Avoid common navigation mistakes.

---

## 📊 Mission Progress

```
████████████░░░░

✔ Operating System

✔ Current User

✔ Hostname

✔ Current Directory

○ Directory Contents

○ Disk Space
```

# 📍 Scene 5 – What's in This Directory?

---

## 💭 The Situation

You've confirmed:

- The operating system.
- Your user account.
- The server.
- Your current location.

Your manager now asks:

> **"The deployment package has already been copied to the server. Before we continue, take a look at what's available in the application directory."**

You navigate to the directory.

Before opening any files, editing configurations, or executing scripts, you ask:

> **"What exactly is in this directory?"**

Experienced engineers always inspect their surroundings before making changes.

---

## ❓ Question

**How do I view the contents of a directory?**

---

# 🔧 Tool — `ls`

### Purpose

List the contents of a directory.

### Syntax

```
ls [options] [directory]
```

---

## ☁ AWS Example

```
ls
```

Example Output

```
deploy.sh
docker-compose.yml
logs
nginx.conf
README.md
```

At first glance, this tells you what resources are available in your current directory.

---

# 💡 Why Does This Matter?

Imagine your manager says:

> **"Execute the deployment script."**

Before running

```
./deploy.sh
```

first verify that the file actually exists.

```
ls
```

A simple check can prevent unnecessary troubleshooting.

---

# 🚀 Tool Evolution

## Level 1 — Basic Listing

```
ls
```

Displays visible files and directories.

---

## Level 2 — Detailed Listing

```
ls -l
```

Example

```
-rwxr-xr-x 1 ec2-user ec2-user 1450 deploy.sh
-rw-r--r-- 1 ec2-user ec2-user 2048 nginx.conf
drwxr-xr-x 2 ec2-user ec2-user 4096 logs
```

The output includes:

- File permissions
- Number of links
- Owner
- Group
- File size
- Last modified date
- File name

This is one of the most frequently used options in Linux administration.

---

## Level 3 — Show Hidden Files

```
ls -a
```

Example

```
.
..
.bashrc
.profile
.git
.ssh
deploy.sh
```

Files beginning with a dot (`.`) are hidden by default.

Many Linux configuration files fall into this category.

---

## Level 4 — Human Readable File Sizes

```
ls -lh
```

Instead of

```
2097152
```

you'll see

```
2.0M
```

This makes file sizes much easier to interpret.

---

## Level 5 — The Administrator's Favorite

```
ls -lah
```

This combines:

- `-l` → Detailed listing
- `-a` → Hidden files
- `-h` → Human-readable sizes

If there's one variation of `ls` you remember, make it this one.

---

# 💡 Understanding File Permissions

Look at this line:

```
-rwxr-xr-x
```

Although you'll learn Linux permissions in detail later, here's a quick overview.

The first character indicates the file type.

|Symbol|Meaning|
|---|---|
|`-`|Regular file|
|`d`|Directory|
|`l`|Symbolic link|

The remaining characters represent permissions for:

- Owner
- Group
- Others

Don't worry about memorizing them yet.

For now, simply recognize that `ls -l` reveals far more than just file names.

We'll revisit permissions in a dedicated mission.

---

# ☁ AWS Scenario

You've connected to a production EC2 instance.

The deployment guide says:

> **"Update the environment variables before restarting the application."**

Instead of immediately editing files, you inspect the directory.

```
ls -lah
```

Output

```
.env
docker-compose.yml
deploy.sh
logs/
README.md
```

You immediately notice:

- The deployment script exists.
- The `.env` file is present.
- The logs directory already exists.

Without opening a single file, you've already learned a great deal about the application.

---

# 🎤 Interview Corner

**Question**

You've connected to an unfamiliar Linux server.

Which `ls` command would you use to display:

- hidden files,
- permissions,
- ownership,
- and human-readable file sizes?

**Expected Discussion**

A strong candidate would answer:

```
ls -lah
```

Then explain:

- `-l` provides the long listing format.
- `-a` displays hidden files.
- `-h` displays human-readable file sizes.

Interviewers appreciate candidates who explain their reasoning—not just the command.

---

# ❌ Common Errors → Root Cause

|Problem|First Thing to Check|
|---|---|
|Can't find a configuration file|Run `ls -a`; it may be hidden.|
|Script isn't present|Verify the current directory using `pwd`, then run `ls`.|
|Unsure who owns a file|Use `ls -l`.|
|File size is difficult to interpret|Use `ls -lh`.|

---

# 🧠 Knowledge Check

### 1. Which command displays hidden files?

- ☐ `ls`
- ☐ `ls -a`
- ☐ `pwd`
- ☐ `hostname`

---

### 2. Which option displays file permissions?

- ☐ `-h`
- ☐ `-a`
- ☐ `-l`
- ☐ `-r`

---

### 3. Which command combines detailed output, hidden files, and human-readable sizes?

- ☐ `ls -l`
- ☐ `ls -ah`
- ☐ `ls -lah`
- ☐ `ls -lh`

---

# ⭐ Engineering Habit

> **Inspect a directory before modifying its contents.**

Never assume a file exists—or that you're looking at the complete picture.

---

# 🛠 Production Tip

One of the first commands many Linux administrators execute after changing directories is:

```
ls -lah
```

It provides an immediate snapshot of:

- Hidden configuration files
- File ownership
- Permissions
- Directory structure
- File sizes

Develop this habit early. It will save you time throughout your career.

---

# ✅ Scene Summary

By completing this scene, you learned how to:

- List directory contents.
- Display detailed file information.
- View hidden files.
- Read file sizes more easily.
- Perform an initial inspection of an unfamiliar directory.

---

## 📊 Mission Progress

```
██████████████░░

✔ Operating System

✔ Current User

✔ Hostname

✔ Current Directory

✔ Directory Contents

○ Disk Space
```

# 📍 Scene 6 – Is There Enough Disk Space?

---

## 💭 The Situation

You've now verified:

- The operating system.
- The Linux kernel.
- Your user account.
- The server.
- Your current location.
- The files available.

Everything looks ready.

You start the deployment.

A few moments later, the deployment pipeline fails.

The error message reads:

```
No space left on device
```

Your first instinct might be to restart the application.

Experienced engineers think differently.

They ask:

> **"Does this server have enough free disk space?"**

Infrastructure problems often appear as application problems.

Always verify the system before blaming the application.

---

## ❓ Question

**How much free disk space is available on this server?**

---

# 🔧 Tool — `df`

### Purpose

Display disk space usage for mounted file systems.

### Syntax

```
df [options]
```

---

## ☁ AWS Example

```
df -h
```

Example Output

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       20G   11G    8G   58% /
tmpfs           975M     0  975M    0% /dev/shm
```

The **`-h`** option displays values in a human-readable format such as KB, MB, and GB.

---

# 💡 Understanding the Output

Let's understand each column.

|Column|Meaning|
|---|---|
|Filesystem|Storage device or partition|
|Size|Total capacity|
|Used|Space already consumed|
|Avail|Remaining free space|
|Use%|Percentage of disk used|
|Mounted on|Mount point|

Among these, **Use%** is often the first column an engineer checks.

If the filesystem approaches **100%**, applications may begin to fail.

---

# 🚀 Tool Evolution

Display disk usage

```
df
```

---

Display human-readable output

```
df -h
```

---

Display only the root filesystem

```
df -h /
```

Useful when you're interested only in the root partition.

---

Display filesystem type

```
df -Th
```

Example

```
Filesystem     Type  Size Used Avail Use% Mounted on
/dev/xvda1     xfs    20G 11G   8G   58% /
```

This tells you whether the filesystem is:

- ext4
- xfs
- btrfs
- tmpfs

Knowing the filesystem type becomes useful during storage troubleshooting and performance tuning.

---

## ☁ AWS Scenario

CloudWatch sends the following alert.

> **Deployment Failed**

You SSH into the EC2 instance.

Instead of restarting the application, you investigate the server.

```
df -h
```

Output

```
Filesystem      Size Used Avail Use%
/dev/xvda1       20G 20G     0 100%
```

You've found the problem.

The filesystem is completely full.

No application can continue writing logs, temporary files, or uploaded content.

The deployment didn't fail because of the application.

It failed because the infrastructure had no remaining storage.

---

## 💡 Looking Ahead

At this point, you've answered:

> **Do I have enough disk space?**

The next logical question becomes:

> **What's consuming all the space?**

That question is answered by another command:

```
du
```

We'll explore `du` in the next mission, where you'll learn how to identify directories and files consuming the most storage.

---

# 🎤 Interview Corner

**Question**

A production application suddenly stops writing log files.

Which command would you execute first?

**Expected Discussion**

A strong candidate might answer:

> "I'd begin by checking the available disk space using `df -h`. If the filesystem is full, the application won't be able to create new log files or temporary files. I'd verify the infrastructure before troubleshooting the application."

Notice the thought process.

The candidate forms a hypothesis before executing commands.

That's what interviewers look for.

---

# ❌ Common Errors → Root Cause

|Problem|First Thing to Check|
|---|---|
|**No space left on device**|`df -h`|
|Logs stopped growing|Check available disk space|
|Deployment failed unexpectedly|Verify the root filesystem isn't full|
|Application cannot create files|Check `Use%` in `df -h`|

---

# 🧠 Knowledge Check

### 1. Which command displays disk usage in a human-readable format?

- ☐ `du -h`
- ☐ `df -h`
- ☐ `ls -lh`
- ☐ `pwd`

---

### 2. Which column deserves immediate attention when checking disk usage?

- ☐ Filesystem
- ☐ Mounted on
- ☐ Use%
- ☐ Size

---

### 3. Which command also displays the filesystem type?

- ☐ `df -h`
- ☐ `df -Th`
- ☐ `pwd`
- ☐ `hostname`

---

# ⭐ Engineering Habit

> **Verify system resources before troubleshooting the application.**

Applications often report symptoms.

Infrastructure often contains the cause.

---

# 🛠 Production Tip

When responding to a production incident, many engineers start with a quick health check:

```
hostname
whoami
pwd
df -h
```

These four commands quickly answer:

- Which server am I on?
- Which user am I?
- Where am I?
- Does the system have enough storage?

Make this your standard routine whenever you log into an unfamiliar Linux server.

---

# ✅ Scene Summary

By completing this scene, you learned how to:

- Check available disk space.
- Interpret `df -h` output.
- Understand filesystem utilization.
- Recognize storage-related application failures.
- Develop a systematic troubleshooting approach.

---

## 📊 Mission Progress

```
████████████████

✔ Operating System

✔ Current User

✔ Hostname

✔ Current Directory

✔ Directory Contents

✔ Disk Space
```

---

## 🎉 Mission 1 Complete

You've now completed your **initial server assessment**.

Without changing a single configuration file or restarting a single service, you've learned how to answer the six most important questions every Linux engineer should ask after logging into a new server.

In the next section, we'll wrap up the mission with:

- 🎯 Mission Debrief
- 🧪 Hands-on Lab
- 🎤 Interview Challenge
- 📌 Key Takeaways
- 🏆 Mission Scorecard
- 🚀 Preview of Mission 2


# 🎯 Mission Debrief

Congratulations, Engineer!

You've successfully completed your first mission.

When you first connected to the EC2 instance, you knew nothing about the environment.

You didn't know:

- Which Linux distribution was running.
- Which kernel version was installed.
- Which user you were logged in as.
- Which server you were connected to.
- Where you were in the filesystem.
- Which files were available.
- Whether the server had enough free disk space.

Instead of making assumptions, you investigated the environment one step at a time.

This is exactly how experienced Linux engineers approach an unfamiliar system.

Notice something important.

Throughout this mission, you were never asked:

> **"Which Linux command do you remember?"**

Instead, every command answered a specific question.

|Question|Command|
|---|---|
|Which operating system is running?|`cat /etc/os-release`|
|Which kernel is running?|`uname -a`|
|Who am I?|`whoami`|
|What permissions do I have?|`id`, `groups`, `sudo -l`|
|Which server is this?|`hostname`, `hostnamectl`|
|Where am I?|`pwd`|
|What files are available?|`ls -lah`|
|Is there enough free disk space?|`df -h`|

This is the mindset that separates engineers who **memorize commands** from engineers who **solve problems**.

---

# ⭐ Engineering Principle

Throughout this book, remember one simple workflow.

```
Observe
      ↓
Think
      ↓
Verify
      ↓
Act
```

Never reverse the order.

Production incidents often begin with someone acting before verifying.

Professional engineers investigate first.

---

# 🧪 Hands-on Lab

Now it's your turn.

Launch an Ubuntu, Amazon Linux, or RHEL EC2 instance and complete the following tasks **without referring to this chapter**.

---

## Task 1 — Identify the Operating System

Expected Command

```
cat /etc/os-release
```

Record:

- Distribution
- Version
- Package Manager

---

## Task 2 — Identify the Kernel

Expected Command

```
uname -a
```

Record:

- Kernel Version
- Architecture

---

## Task 3 — Identify the Current User

Expected Commands

```
whoami
id
groups
```

Record:

- Username
- UID
- Groups

---

## Task 4 — Check Your Sudo Permissions

Expected Command

```
sudo -l
```

Record:

- Can your user execute all commands?
- Are there any restrictions?

---

## Task 5 — Verify the Server

Expected Commands

```
hostname
hostnamectl
```

If you're using AWS, compare the hostname with the EC2 instance details in the AWS Management Console.

---

## Task 6 — Determine Your Location

Expected Commands

```
pwd
cd /
pwd
cd ~
pwd
cd -
pwd
```

Observe how your working directory changes.

---

## Task 7 — Inspect the Directory

Expected Commands

```
ls
ls -l
ls -a
ls -lh
ls -lah
```

Try to identify:

- Hidden files
- Directories
- File sizes
- File permissions

---

## Task 8 — Check Disk Space

Expected Commands

```
df -h
df -Th
```

Answer:

- Which filesystem contains the root directory?
- Which filesystem type is being used?
- How much free space remains?

---

# 🧩 Mini Challenge

Without looking back at the chapter, answer the following questions.

1. Which command tells you which Linux distribution is running?
2. Which command displays the kernel version?
3. Which command tells you who you are logged in as?
4. Which command lists your Linux groups?
5. Which command displays the hostname?
6. Which command tells you your current directory?
7. Which command displays hidden files?
8. Which command checks available disk space?

If you can answer all eight confidently, you've mastered the fundamentals covered in this mission.

---

# 🎤 Interview Challenge

Imagine you're attending an AWS/DevOps interview.

The interviewer says:

> **"You've just SSH'd into a Linux server you've never seen before. You are not allowed to change anything. Walk me through your first five minutes."**

Pause here.

Answer aloud before reading further.

---

## Sample Answer

> "My first step is to understand the environment before making any changes.
> 
> I'd identify the operating system using `cat /etc/os-release`, then check the kernel version and architecture with `uname -a`.
> 
> Next, I'd verify the current user using `whoami`, inspect the user's permissions with `id`, `groups`, and `sudo -l`, then confirm the hostname using `hostname` or `hostnamectl`.
> 
> I'd check my current working directory with `pwd`, inspect the available files using `ls -lah`, and finally verify the available disk space with `df -h`.
> 
> Only after understanding the environment would I begin troubleshooting or making changes."

Notice how this answer demonstrates a **methodical approach**, not just command memorization.

---

# 📌 Key Takeaways

By completing this mission, you can now:

- ✅ Identify the Linux distribution.
- ✅ Identify the Linux kernel and architecture.
- ✅ Determine the current user.
- ✅ Understand user permissions.
- ✅ Verify the server identity.
- ✅ Navigate the filesystem.
- ✅ Inspect files and directories.
- ✅ Check available disk space.

More importantly, you've developed a repeatable process for approaching an unfamiliar Linux server.

---

# 🏆 Mission Scorecard

Rate your confidence before moving to Mission 2.

|Skill|⭐⭐⭐⭐⭐|
|---|---|
|Identify the operating system|☐ ☐ ☐ ☐ ☐|
|Understand the kernel|☐ ☐ ☐ ☐ ☐|
|Verify user identity|☐ ☐ ☐ ☐ ☐|
|Understand permissions|☐ ☐ ☐ ☐ ☐|
|Verify server identity|☐ ☐ ☐ ☐ ☐|
|Navigate the filesystem|☐ ☐ ☐ ☐ ☐|
|Inspect directories|☐ ☐ ☐ ☐ ☐|
|Check disk usage|☐ ☐ ☐ ☐ ☐|

If any skill scores below **★★★★☆**, spend a little more time practicing before moving on.

---

# 🚀 Next Mission

You've learned **how to understand a server**.

Now it's time to **explore it**.

In **Mission 2 – Explore the Linux File System**, you'll learn how to:

- Navigate efficiently using `cd`.
- Understand the Linux Filesystem Hierarchy Standard (FHS).
- Create and organize directories with `mkdir`.
- Create files using `touch`.
- Copy, move, and rename files using `cp` and `mv`.
- Remove files and directories safely using `rm` and `rmdir`.

By the end of the next mission, you'll be comfortable moving through almost any Linux server.

---

# 🏁 Mission Complete

> **Mission Status:** ✅ SUCCESS

Your manager reviews your work and nods.

> **"Good."**

> **"You didn't restart any services."**

> **"You didn't edit any configuration files."**

> **"You didn't make assumptions."**

Instead, you took the time to understand the system before acting.

That's exactly what professional Linux engineers do.

He smiles and hands you the next assignment.

> **"Let's explore the filesystem."**

Turn the page.

**Mission 2 awaits.**