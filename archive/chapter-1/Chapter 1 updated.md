# Chapter 1

# 🖥️ Mission 1 – Meet Your Server

---

> ## 🎯 Mission Brief
> 
> Welcome to your first day as a Cloud Engineer.
> 
> Your manager has provisioned an Amazon EC2 instance and shared the SSH credentials. The server hosts an internal web application used by several teams.
> 
> As you prepare to investigate the system, your manager stops you and says:
> 
> **"Don't change anything. First, understand the server."**
> 
> It sounds like simple advice, but it reflects one of the most important habits of experienced Linux engineers.
> 
> Beginners often log into a server and immediately start installing packages, restarting services, or editing configuration files.
> 
> Experienced engineers do something different.
> 
> They spend the first few minutes answering a series of questions:
> 
> - What operating system is this?
> - Which user am I logged in as?
> - Is this the correct server?
> - Where am I in the filesystem?
> - What files are available?
> - Does the server have enough free disk space?
> 
> Only after answering these questions do they make changes.
> 
> Throughout this mission, you'll develop the same habit.
> 
> By the end of the chapter, you'll know how to investigate an unfamiliar Linux server with confidence—without making a single modification.

---

# 🎯 Learning Objectives

By the end of this mission, you'll be able to:

- ✔ Identify the Linux distribution running on a server.
- ✔ Determine the currently logged-in user.
- ✔ Verify the hostname.
- ✔ Find your current working directory.
- ✔ Inspect files and directories.
- ✔ Check available disk space.
- ✔ Develop the engineering mindset of **Observe → Think → Verify → Act**.

---

# 📍 Scene 1 – Which Operating System Is Running?

---

## 💭 The Situation

You have successfully connected to your EC2 instance.

The terminal displays:

```
[ec2-user@ip-10-0-1-25 ~]$
```

The cursor is blinking.

The server is waiting for your next command.

Many beginners immediately try to install software or restart services.

You don't.

Before doing anything, you ask yourself:

> **"What operating system is this server running?"**

The answer determines which package manager to use, where configuration files are stored, and sometimes even which commands are available.

Never assume.

Always verify.

---

## 🔧 Tool – `cat`

### Purpose

Display the contents of a text file.

### Syntax

```
cat <filename>
```

---

## ☁ AWS Example

```
cat /etc/os-release
```

### Sample Output – Amazon Linux 2023

```
NAME="Amazon Linux"
VERSION="2023"
ID="amzn"
```

### Sample Output – Ubuntu

```
NAME="Ubuntu"
VERSION="24.04 LTS"
ID=ubuntu
```

### Sample Output – Red Hat Enterprise Linux

```
NAME="Red Hat Enterprise Linux"
VERSION="9.x"
ID="rhel"
```

---

## 💡 Why Does This Matter?

Suppose your manager asks you to install Nginx.

You immediately type:

```
sudo apt install nginx
```

The command fails.

Why?

Because the server is running Amazon Linux, not Ubuntu.

Amazon Linux uses:

```
sudo dnf install nginx
```

or, on Amazon Linux 2,

```
sudo yum install nginx
```

The problem wasn't your Linux knowledge.

The problem was making an assumption.

---

## 📘 Package Managers

|Linux Distribution|Package Manager|
|---|---|
|Ubuntu|`apt`|
|Amazon Linux 2023|`dnf`|
|Amazon Linux 2|`yum`|
|Red Hat Enterprise Linux|`dnf`|

---

## 🚀 Tool Evolution

The `cat` command appears simple, but you'll use it throughout your Linux career.

View operating system information:

```
cat /etc/os-release
```

View the hosts file:

```
cat /etc/hosts
```

View mounted file systems:

```
cat /etc/fstab
```

View local user accounts:

```
cat /etc/passwd
```

One command.

Many uses.

---

## 🎤 Interview Corner

**Question**

You've just connected to an unfamiliar EC2 instance.

What would be the first thing you verify?

**Expected Discussion**

A strong candidate might answer:

> "I'd first identify the operating system using `cat /etc/os-release`. That tells me which package manager and system conventions the server uses before I make any changes."

Notice that the answer emphasizes **thinking before acting**, not simply recalling a command.

---

## ⭐ Engineering Habit

> **Never assume the operating system.**
> 
> **Verify it first.**

---

## ✅ Scene Summary

You learned:

- Why identifying the operating system is your first step.
- How to use `cat /etc/os-release`.
- Why different Linux distributions use different package managers.
- How experienced engineers avoid assumptions.

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

You now know the operating system.

Your next question is just as important:

> **"Which user account am I using?"**

Every action you perform on Linux happens as a user.

Your permissions determine which files you can read, which services you can restart, and whether administrative commands will succeed.

Before troubleshooting permission issues, first identify yourself.

---

## 🔧 Tool – `whoami`

### Purpose

Display the username of the currently logged-in user.

### Syntax

```
whoami
```

---

## ☁ AWS Example

On an Amazon Linux EC2 instance:

```
whoami
```

Output:

```
ec2-user
```

On Ubuntu:

```
ubuntu
```

On many RHEL EC2 images:

```
ec2-user
```

> **Note:** The default login user depends on the Amazon Machine Image (AMI) used to launch the instance.

---

## 📘 Common Default Users

|AMI|Default User|
|---|---|
|Ubuntu|`ubuntu`|
|Amazon Linux|`ec2-user`|
|RHEL|`ec2-user`|

---

## 💡 Why Does This Matter?

Imagine you're asked to update the Nginx configuration.

You run:

```
sudo vi /etc/nginx/nginx.conf
```

Instead of editing the file, you receive a permission-related error.

Before assuming something is wrong with the server, ask:

> **Which user am I logged in as?**

Understanding your identity is often the first step in solving permission problems.

---

## 🚀 Tool Evolution

Display the current user:

```
whoami
```

Display your User ID (UID), Group ID (GID), and group memberships:

```
id
```

Example:

```
uid=1000(ec2-user) gid=1000(ec2-user) groups=1000(ec2-user),10(wheel)
```

Display only your groups:

```
groups
```

Example:

```
ec2-user wheel docker
```

These commands help you understand not just _who_ you are, but also _what you're allowed to do_.

---

## ☁ AWS Scenario

A teammate says:

> "Docker is already installed, but `docker ps` keeps returning a permission denied error."

Before reinstalling Docker or changing any configuration, you check:

- Which user am I?
- Am I a member of the `docker` group?

Sometimes the solution is as simple as adding the user to the correct group.

---

## 🎤 Interview Corner

**Question**

You've logged into an EC2 instance, but commands requiring elevated privileges are failing.

What would you verify first?

**Expected Discussion**

> "I'd check the current user using `whoami` and inspect the user's groups using `id` or `groups`. This helps determine whether the issue is related to permissions before attempting any fixes."

---

## ⭐ Engineering Habit

> **Always know your identity before changing a system.**

Permissions are tied to users and groups—not intentions.

---

## ✅ Scene Summary

You learned:

- How to identify the current user.
- Why user identity affects permissions.
- How to inspect group memberships.
- How experienced engineers troubleshoot permission issues logically.

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

You've identified the operating system and confirmed your user account.

Now your manager sends a message:

> **"Please restart the web service on the production server."**

You have several SSH sessions open.

Development.

Testing.

Production.

Before touching anything, you pause and ask:

> **"Am I connected to the correct server?"**

This simple verification has prevented countless production incidents.

---

## 🔧 Tool – `hostname`

### Purpose

Display the hostname of the current Linux system.

### Syntax

```
hostname
```

---

## ☁ AWS Example

```
hostname
```

Sample Output

```
ip-10-0-1-25
```

This hostname uniquely identifies the EC2 instance within your environment.

---

## 🔧 Tool – `hostnamectl`

### Purpose

Display detailed information about the system.

### Syntax

```
hostnamectl
```

Sample Output

```
Static hostname: ip-10-0-1-25
Operating System: Amazon Linux 2023
Kernel: Linux 6.x
Architecture: x86_64
```

Unlike `hostname`, this command provides a broader picture of the system.

---

## 💡 Why Does This Matter?

Imagine you're responsible for these servers.

|Hostname|Purpose|
|---|---|
|web-prod-01|Production Web Server|
|web-test-01|Testing Web Server|
|db-prod-01|Production Database|

You intend to restart Nginx.

But you're actually connected to the database server.

One wrong command could interrupt production.

Professional engineers verify the server before making changes.

---

## 🚀 Tool Evolution

Display hostname

```
hostname
```

Display detailed system information

```
hostnamectl
```

Display the Fully Qualified Domain Name

```
hostname -f
```

Example

```
web-prod-01.company.internal
```

Display assigned IP addresses

```
hostname -I
```

Example

```
10.0.1.25 172.31.5.10
```

---

## ☁ AWS Scenario

An alert appears.

> **High CPU Utilization on Production Web Server**

You SSH into an instance.

Before investigating CPU usage, you verify:

```
hostname
```

The hostname shows:

```
web-test-01
```

You immediately realize you're connected to the wrong server.

You reconnect to the correct instance before continuing.

That thirty-second verification saved you from wasting valuable troubleshooting time.

---

## 🎤 Interview Corner

**Question**

Why would you execute `hostname` immediately after logging into an EC2 instance?

**Expected Discussion**

> "I want to confirm that I'm connected to the intended server before making administrative changes. In environments with multiple EC2 instances, verifying the hostname helps avoid mistakes."

---

## ⭐ Engineering Habit

> **Never assume you're on the correct server. Verify it.**

---

## ✅ Scene Summary

You learned:

- How to identify the current server.
- The difference between `hostname` and `hostnamectl`.
- Why verifying the server prevents production mistakes.

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

---

# 📍 Scene 4 – Where Am I?

---

## 💭 The Situation

You've confirmed:

- The operating system.
- Your user account.
- The server.

Now imagine your manager asks:

> **"Please update the application's configuration file."**

You type:

```
cat config.yaml
```

Linux responds:

```
cat: config.yaml: No such file or directory
```

Did the file disappear?

Not necessarily.

The first question should be:

> **"Where am I?"**

---

## 🔧 Tool – `pwd`

### Purpose

Display the absolute path of your current working directory.

### Syntax

```
pwd
```

---

## ☁ AWS Example

```
pwd
```

Output

```
/home/ec2-user
```

or

```
/home/ubuntu
```

depending on the AMI.

---

## 💡 Why Does This Matter?

Linux commands often work relative to your current directory.

Suppose you execute:

```
cat nginx.conf
```

Will the command work?

It depends.

If `nginx.conf` exists in your current directory, Linux displays its contents.

Otherwise, you'll see:

```
No such file or directory
```

The problem isn't always the file.

Sometimes it's simply your location.

---

## 🚀 Tool Evolution

Display your current directory

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

This is one of those small commands that becomes invaluable during daily administration.

---

## ☁ AWS Scenario

You're following deployment documentation.

Step three says:

> **Run `deploy.sh` from the application's root directory.**

Instead of immediately running:

```
./deploy.sh
```

you verify your location.

```
pwd
```

The output shows:

```
/home/ec2-user
```

The application actually resides in:

```
/opt/webapp
```

You navigate there first.

A simple verification prevents a failed deployment.

---

## 🎤 Interview Corner

**Question**

You execute:

```
cat app.conf
```

Linux returns:

```
No such file or directory
```

What would you check first?

**Expected Discussion**

> "I'd first check my current working directory using `pwd`. If I'm in the wrong location, I'll navigate to the appropriate directory before assuming the file is missing."

This demonstrates a logical troubleshooting approach.

---

## ⭐ Engineering Habit

> **Always know where you are before creating, editing, or deleting files.**

---

## ✅ Scene Summary

You learned:

- How to determine your current working directory.
- Why many file-related errors are actually navigation issues.
- How `pwd` and `cd` work together.
- Why location matters before executing commands.

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

You know:

- Which operating system you're using.
- Which user you're logged in as.
- Which server you're connected to.
- Your current location.

Now your manager says:

> **"The deployment files are already on the server."**

Before opening, editing, or executing anything, you need to answer one question:

> **"What files and folders are available here?"**

Experienced engineers always inspect the directory before taking action.

---

## 🔧 Tool – `ls`

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

Sample Output

```
Documents
Downloads
logs
scripts
webapp
```

This gives you a quick overview of the current directory.

---

## 💡 Why Does This Matter?

Suppose your manager says:

> **"Run the deployment script in the current directory."**

Before executing:

```
./deploy.sh
```

verify that the file actually exists.

```
ls
```

Never assume.

Always verify.

---

# 🚀 Tool Evolution

### Level 1 – Basic Listing

```
ls
```

Lists visible files and directories.

---

### Level 2 – Detailed Listing

```
ls -l
```

Example

```
-rw-r--r-- 1 ec2-user ec2-user 2048 app.log
drwxr-xr-x 2 ec2-user ec2-user 4096 scripts
```

Now you can see:

- File permissions
- Owner
- Group
- File size
- Last modified date

---

### Level 3 – Show Hidden Files

```
ls -a
```

Example

```
.
..
.bashrc
.profile
.ssh
.git
```

Files beginning with a dot (`.`) are hidden by default.

Many important Linux configuration files fall into this category.

---

### Level 4 – Human Readable Sizes

```
ls -lh
```

Instead of

```
2097152
```

You'll see

```
2.0M
```

Much easier to read.

---

### Level 5 – The Daily Command

```
ls -lah
```

This is one of the most commonly used combinations by Linux administrators because it shows:

- Hidden files
- Permissions
- Ownership
- Human-readable sizes

If you remember one variation of `ls`, make it this one.

---

## ☁ AWS Scenario

You've connected to a production EC2 instance.

The application isn't starting.

Before opening configuration files, you inspect the directory.

```
ls -lah
```

Output

```
.env
docker-compose.yml
nginx.conf
logs/
README.md
```

Immediately you notice that the `.env` file exists.

Your investigation can now continue in the right direction.

---

## 🎤 Interview Corner

**Question**

You've connected to an unfamiliar Linux server.

How would you inspect the current directory, including hidden files and permissions?

**Expected Discussion**

```
ls -lah
```

A good candidate explains:

- `-l` → Long listing
- `-a` → Hidden files
- `-h` → Human-readable file sizes

Interviewers value explanations, not just commands.

---

## ⭐ Engineering Habit

> **Inspect the directory before modifying its contents.**

---

## ✅ Scene Summary

You learned:

- How to list directory contents.
- How to display detailed file information.
- How to view hidden files.
- How to interpret permissions and file sizes.

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

---

# 📍 Scene 6 – Is There Enough Disk Space?

---

## 💭 The Situation

You've confirmed:

- The operating system.
- Your user account.
- The server.
- Your location.
- The available files.

Just as you're about to deploy the application, the deployment pipeline fails with the message:

> **No space left on device**

Before blaming the application, ask yourself:

> **"Does this server have enough free disk space?"**

---

## 🔧 Tool – `df`

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

Example

```
Filesystem      Size Used Avail Use% Mounted on
/dev/xvda1       20G 11G   8G   58% /
tmpfs           975M   0 975M    0% /dev/shm
```

The `-h` option displays values in a human-readable format.

---

## 💡 Understanding the Output

|Column|Meaning|
|---|---|
|Filesystem|Storage device|
|Size|Total capacity|
|Used|Used space|
|Avail|Available space|
|Use%|Percentage used|
|Mounted on|Mount point|

The **Use%** column deserves your immediate attention.

---

## 🚀 Tool Evolution

Display human-readable disk usage

```
df -h
```

---

Display a specific filesystem

```
df -h /
```

---

Display filesystem type

```
df -Th
```

Example

```
Filesystem Type Size Used Avail Use% Mounted on
/dev/xvda1 xfs 20G 11G 8G 58% /
```

This helps identify whether the filesystem is:

- xfs
- ext4
- ext3

---

## ☁ AWS Scenario

CloudWatch sends an alert:

> **Deployment Failed**

SSH into the EC2 instance.

Your first command:

```
df -h
```

Output

```
Filesystem      Size Used Avail Use%
/dev/xvda1       20G 20G     0 100%
```

The application isn't the problem.

The filesystem is completely full.

Before restarting services or redeploying applications, resolve the storage issue.

In the next mission, you'll learn how to identify which files and directories are consuming disk space.

---

## 🎤 Interview Corner

**Question**

A production application suddenly stops writing log files.

Which Linux command would you execute first?

**Expected Discussion**

> "I'd first check the available disk space using `df -h`. If the filesystem is full, applications may fail to write logs or temporary files."

Notice that you're testing a hypothesis before taking action.

---

## ⭐ Engineering Habit

> **Check system resources before blaming the application.**

---

## ✅ Scene Summary

You learned:

- How to check available disk space.
- How to interpret `df -h` output.
- Why storage issues often affect application behavior.
- How experienced engineers verify infrastructure before troubleshooting software.

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

# 🎯 Mission Debrief

Congratulations, Engineer!

You have successfully completed your first mission.

When you first connected to the EC2 instance, you knew nothing about the environment.

You didn't know:

- Which operating system was running.
- Which user you were logged in as.
- Which server you were connected to.
- Where you were in the filesystem.
- Which files were available.
- Whether the server had enough free disk space.

Instead of making assumptions, you investigated the system step by step.

This is exactly how experienced Linux engineers work.

Notice something important.

Throughout this mission, you were never asked:

> **"Which Linux command do you remember?"**

Instead, every command answered a question.

|Question|Command|
|---|---|
|Which operating system is running?|`cat /etc/os-release`|
|Who am I?|`whoami`|
|Which server am I connected to?|`hostname`, `hostnamectl`|
|Where am I?|`pwd`|
|What files are available?|`ls -lah`|
|Is there enough disk space?|`df -h`|

That's the mindset we want to develop throughout this book.

Think first.

Then choose the right command.

---

# ⭐ Engineering Principle

> **Observe. Think. Verify. Act.**

Every experienced engineer follows this sequence.

Observe the environment.

Think before making assumptions.

Verify using commands.

Only then take action.

This principle will guide every mission in this book.

---

# 🧪 Hands-on Lab

Launch an Ubuntu, Amazon Linux, or RHEL EC2 instance.

Without making any configuration changes, perform the following tasks.

### Task 1

Identify the operating system.

Expected command:

```
cat /etc/os-release
```

---

### Task 2

Determine the current user.

Expected command:

```
whoami
```

---

### Task 3

Display the hostname.

Expected command:

```
hostname
```

---

### Task 4

Determine your current working directory.

Expected command:

```
pwd
```

---

### Task 5

List all files, including hidden files.

Expected command:

```
ls -lah
```

---

### Task 6

Check the available disk space.

Expected command:

```
df -h
```

---

### Bonus Challenge

Write down your observations.

- Which Linux distribution are you using?
- What is the default login user?
- What is the hostname?
- Which directory did you start in?
- Were any hidden files present?
- How much free disk space is available?

---

# 🎤 Interview Challenge

Imagine you're attending an AWS/DevOps interview.

The interviewer says:

> "You've just connected to an EC2 instance you've never seen before.

> You are not allowed to change anything.

> Walk me through the first commands you would execute and explain why."

Take a few minutes and answer aloud before reading further.

---

## Sample Discussion

A strong answer might sound like this:

> "First, I'd identify the operating system using `cat /etc/os-release` so I know which package manager and conventions the system uses.

> Next, I'd verify the current user with `whoami` because permissions depend on user identity.

> I'd confirm the hostname using `hostname` to ensure I'm connected to the intended server.

> Then I'd determine my current working directory with `pwd`.

> I'd inspect the available files using `ls -lah`.

> Finally, I'd check the available disk space using `df -h` before performing any deployment or troubleshooting."

Notice that every command has a purpose.

Interviewers value your reasoning more than your memory.

---

# 📌 Key Takeaways

By completing this mission, you have learned to:

✅ Identify the Linux distribution.

✅ Verify the logged-in user.

✅ Confirm the hostname.

✅ Determine your current location.

✅ Inspect directory contents.

✅ Check available disk space.

More importantly, you've learned to investigate a system before making changes.

That habit will serve you throughout your career.

---

# 📚 Commands Learned

|Command|Purpose|
|---|---|
|`cat /etc/os-release`|Identify the operating system|
|`whoami`|Display the current user|
|`id`|Display user and group information|
|`groups`|Display group memberships|
|`hostname`|Display the hostname|
|`hostnamectl`|Display detailed system information|
|`pwd`|Display the current directory|
|`cd`|Change directories|
|`ls`|List directory contents|
|`ls -lah`|Detailed listing with hidden files|
|`df -h`|Check disk usage|
|`df -Th`|Display filesystem type|

---

# 🚀 Next Mission

Mission 1 helped you understand **where you are**.

Mission 2 will teach you **how to move around the Linux filesystem with confidence**.

You'll learn how to:

- Navigate directories.
- Create files and folders.
- Copy and move files.
- Rename files.
- Delete files safely.
- Understand the Linux filesystem hierarchy.

By the end of the next mission, you'll feel comfortable navigating almost any Linux server.

---

# 🏁 Mission Complete

> **Mission Status:** ✅ SUCCESS

Your manager looks at your terminal and smiles.

> **"Good."**

> **"You didn't change anything."**

> **"That's exactly what I wanted."**

He hands you the next assignment.

> **"Now let's explore the Linux filesystem."**