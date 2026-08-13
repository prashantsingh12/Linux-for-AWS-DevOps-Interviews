

# Mission 1 – Meet Your Server

## Mission Brief

Congratulations!

Today is your first day as a Cloud Engineer.

Your manager has provisioned an Amazon EC2 instance and shared the SSH credentials.

Before deploying applications or making any changes, your manager gives you one instruction:

> **"Don't change anything. First, understand the server."**

Many beginners immediately start installing packages or restarting services.

Experienced engineers don't.

They first observe the environment, gather facts, and build an understanding of the system.

Your mission is to investigate the server without making any changes.

By the end of this mission, you'll know how experienced Linux engineers approach an unfamiliar server.

---

## Learning Objectives

After completing this mission, you'll be able to:

- Identify the Linux distribution.
- Identify the current user.
- Identify the hostname.
- Determine your current working directory.
- Explore files and directories.
- Check available disk space.
- Build the habit of investigating before acting.

---

# Scene 1 – You Have Logged In

You receive the following command from your manager.

```
ssh -i my-key.pem ec2-user@<public-ip>
```

After successful authentication, you see:

```
[ec2-user@ip-10-0-1-25 ~]$
```

The cursor is blinking.

The server is waiting.

**What should you do first?**

Before typing your first command, ask yourself:

> **What do I need to know before I touch this server?**

---

# Question 1

## Which operating system is running?

The answer determines:

- Which package manager to use.
- Which repositories are available.
- Where configuration files may be located.
- Which commands are available.

Never assume.

Always verify.

---

## Tool – `cat`

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

### Sample Output (Amazon Linux)

```
NAME="Amazon Linux"
VERSION="2023"
ID="amzn"
```

### Sample Output (Ubuntu)

```
NAME="Ubuntu"
VERSION="24.04 LTS"
```

### Sample Output (RHEL)

```
NAME="Red Hat Enterprise Linux"
VERSION="9.x"
```

---

## Why does this matter?

Different Linux distributions use different package managers.

|Distribution|Package Manager|
|---|---|
|Ubuntu|apt|
|Amazon Linux 2023|dnf|
|Amazon Linux 2|yum|
|RHEL|dnf / yum|

Suppose your manager asks you to install Nginx.

If you immediately run:

```
sudo apt install nginx
```

the command will fail on Amazon Linux and RHEL.

The problem isn't the command.

The problem is the assumption.

---

## Think Like an Engineer

Experienced engineers don't memorize commands.

They gather information first.

Your first habit should be:

> **Never assume the operating system. Verify it.**

---

## Tool Evolution

You'll use `cat` throughout your Linux journey.

### View the hosts file

```
cat /etc/hosts
```

### View mounted file systems

```
cat /etc/fstab
```

### View user accounts

```
cat /etc/passwd
```

Notice how the same command solves different problems.

---

## Interview Corner

**Interviewer**

You've just connected to an unfamiliar EC2 instance.

What information would you collect before making any changes?

**Expected discussion**

A strong candidate explains their thought process:

> "My first step is to identify the operating system using `cat /etc/os-release`. That tells me which package manager is available and helps me understand the environment before making changes."

---

## Mission Reflection

Today you didn't install software.

You didn't restart a service.

You didn't modify any configuration.

Yet you've completed an important task.

You **understood the environment before acting**.

This habit separates experienced engineers from beginners.

Remember our philosophy:

> **Observe. Think. Verify. Act.**





# Scene 2 – Who Am I on This Server?

You now know the operating system.

Your next question is:

> **Who am I logged in as?**

This may sound like a simple question, but it's one of the first things an experienced engineer verifies.

Your permissions, the files you can access, and the commands you can execute all depend on your current user.

---

## Question 2

### Which user account am I using?

## Tool – `whoami`

### Purpose

Display the username of the currently logged-in user.

### Syntax

```
whoami
```

### AWS Example

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

On RHEL:

```
ec2-user
```

> **Note:** The default login user depends on the Amazon Machine Image (AMI) you launched.

|AMI|Default User|
|---|---|
|Ubuntu|`ubuntu`|
|Amazon Linux|`ec2-user`|
|RHEL|`ec2-user`|

---

## Why does this matter?

Suppose you're trying to edit a configuration file.

```
sudo vi /etc/nginx/nginx.conf
```

Whether this command succeeds depends on the privileges of your current user.

Before troubleshooting permission issues, first confirm who you are.

---

## Think Like an Engineer

A junior engineer often asks:

> "Why am I getting Permission denied?"

A senior engineer first asks:

> "Which user am I logged in as?"

That small difference in thinking saves time.

---

## Tool Evolution

### Verify your username

```
whoami
```

### Find your User ID (UID) and Group ID (GID)

```
id
```

Sample output:

```
uid=1000(ec2-user) gid=1000(ec2-user) groups=1000(ec2-user),10(wheel)
```

### Check all groups you belong to

```
groups
```

Example:

```
ec2-user wheel docker
```

Knowing your groups is useful because permissions are often granted to groups rather than individual users.

---

## AWS/DevOps Scenario

You've joined a project where Docker is already installed.

Running the following command returns:

```
docker ps
```

```
permission denied while trying to connect to the Docker daemon
```

Before reinstalling Docker or changing configurations, ask:

- Which user am I logged in as?
- Am I a member of the `docker` group?

Often, the issue is simply group membership—not Docker itself.

---

## Interview Corner

**Interviewer**

You've SSH'd into an EC2 instance and every command requiring elevated privileges is failing.

What would you check first?

**Expected Discussion**

A good candidate might say:

> "I'd verify the current user using `whoami`, then check the user's groups using `groups` or `id`. That tells me whether the issue is related to permissions or group membership."

Notice that you're investigating before trying random fixes.

---

## Mission Reflection

You now know **who** you are on this server.

Understanding your identity is the first step toward understanding your permissions.

Remember:

> **Every action you perform is executed as a user.**

Before changing a system, always know which identity you're using.




# Scene 3 – Which Server Am I Connected To?

You now know:

- Which operating system is running.
- Which user you're logged in as.

Your next question is:

> **Am I connected to the correct server?**

In AWS environments, it's common to manage dozens or even hundreds of EC2 instances.

Connecting to the wrong server can have serious consequences.

Before making any changes, always verify the server's identity.

---

## Question 3

### Which server am I connected to?

## Tool – `hostname`

### Purpose

Display the hostname of the current system.

### Syntax

```
hostname
```

### AWS Example

```
hostname
```

Sample Output

```
ip-10-0-1-25
```

The hostname uniquely identifies the server within your environment.

---

## Tool – `hostnamectl`

### Purpose

Display detailed information about the system.

### Syntax

```
hostnamectl
```

### Sample Output

```
 Static hostname: ip-10-0-1-25
 Operating System: Amazon Linux 2023
 Kernel: Linux 6.x
 Architecture: x86_64
```

Unlike `hostname`, `hostnamectl` provides additional information including:

- Hostname
- Operating System
- Kernel Version
- Architecture

---

## Why does this matter?

Imagine you're responsible for two production servers.

```
Web Server
ip-10-0-1-25

Database Server
ip-10-0-2-18
```

You intend to restart Nginx on the web server.

But you accidentally SSH into the database server.

Without checking the hostname, you might restart the wrong service on the wrong machine.

Professional engineers verify the server before making changes.

---

## Think Like an Engineer

Never assume.

Always verify:

- Which server is this?
- Is this Development?
- Is this Testing?
- Is this Production?

Many production incidents happen because an engineer connected to the wrong server.

---

## Tool Evolution

### Display only the hostname

```
hostname
```

### Display detailed system information

```
hostnamectl
```

### Display the Fully Qualified Domain Name (FQDN)

```
hostname -f
```

Example:

```
web01.company.internal
```

### Display the IP addresses assigned to the host

```
hostname -I
```

Example:

```
10.0.1.25 172.31.0.15
```

This is useful when troubleshooting networking or verifying the server's assigned IP addresses.

---

## AWS/DevOps Scenario

Your team maintains three environments:

- Development
- Testing
- Production

You receive a message:

> "Please restart the Nginx service on the production web server."

Before running any command, what should you verify?

A good engineer first confirms:

- The hostname.
- The operating system.
- The current user.

Only then should they proceed with administrative tasks.

---

## Interview Corner

**Interviewer**

You've connected to an EC2 instance. How would you verify that you're on the correct server?

**Expected Discussion**

A strong candidate might answer:

> "I'd first check the hostname using `hostname` or `hostnamectl`. I'd also verify the operating system and current user to ensure I'm connected to the intended server before making any changes."

Notice the emphasis on **verification before action**.

---

## Common Mistakes

❌ Assuming the hostname based on the SSH command.

❌ Performing administrative tasks without confirming the environment.

❌ Ignoring whether the server is Development, Testing, or Production.

---

## Mission Reflection

You now know:

- What operating system you're using.
- Who you are.
- Which server you're connected to.

You're slowly building a complete picture of the environment.

That's exactly what experienced engineers do before making changes.



# Scene 4 – Where Am I?

You now know:

- Which operating system is running.
- Which user you're logged in as.
- Which server you're connected to.

Your next question is:

> **Where am I in the file system?**

Just like a traveler checks their location before starting a journey, an engineer should know their current location before navigating the server.

---

## Question 4

### What is my current working directory?

## Tool – `pwd`

### Purpose

Display the absolute path of your current working directory.

### Syntax

```
pwd
```

### AWS Example

```
pwd
```

Sample Output

```
/home/ec2-user
```

or

```
/home/ubuntu
```

Depending on the AMI you're using.

---

## Why does this matter?

Every command you execute works relative to your current location.

Consider the following command:

```
cat config.yaml
```

Will it work?

The answer depends on **where you are**.

If `config.yaml` exists in your current directory, the command succeeds.

Otherwise, you'll receive:

```
cat: config.yaml: No such file or directory
```

Before searching for the file, first ask:

> **Where am I?**

---

## Think Like an Engineer

Suppose your manager says:

> "Please check the application's configuration file."

Instead of immediately typing random commands, first determine your current location.

```
pwd
```

Then decide where to navigate next.

---

## Tool Evolution

### Display the current directory

```
pwd
```

### Move to your home directory

```
cd
```

or

```
cd ~
```

Verify your new location:

```
pwd
```

Expected Output:

```
/home/ec2-user
```

or

```
/home/ubuntu
```

---

### Move to the root directory

```
cd /
pwd
```

Output:

```
/
```

---

### Move back to the previous directory

```
cd -
```

This is one of the most useful but often overlooked commands.

---

## AWS/DevOps Scenario

You're following deployment documentation that says:

> "Run the deployment script from the application's root directory."

Before executing:

```
./deploy.sh
```

Ask yourself:

> **Am I in the correct directory?**

Running deployment scripts from the wrong location can result in missing files, failed deployments, or accidental changes.

---

## Interview Corner

**Interviewer**

You execute:

```
cat nginx.conf
```

and receive:

```
No such file or directory
```

What would you check first?

**Expected Discussion**

A strong candidate might say:

> "I'd first check my current working directory using `pwd`. If I'm in the wrong location, I'll navigate to the correct directory or use the file's absolute path."

This shows you're diagnosing the problem instead of guessing.

---

## Common Mistakes

❌ Assuming you're in your home directory.

❌ Running scripts without confirming the current location.

❌ Using relative paths when an absolute path would be clearer.

---

## Mission Reflection

You now know:

- Which operating system you're using.
- Which user you're logged in as.
- Which server you're connected to.
- Where you are in the file system.

You're building awareness before taking action.

That's a habit every Linux engineer should develop.

---

## Engineering Habit

> **Always know where you are before running commands that create, modify, or delete files.**



# Scene 5 – What's in This Directory?

You know where you are.

Now the next question is:

> **What files and folders are available here?**

Before opening, editing, or deleting anything, first inspect your surroundings.

---

## Question 5

### How do I see the contents of a directory?

## Tool – `ls`

### Purpose

List the contents of a directory.

### Syntax

```
ls [options] [directory]
```

### AWS Example

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

At first glance, this tells you what is available in your current directory.

---

## Why does this matter?

Suppose your manager asks:

> "The deployment script is in this directory. Please execute it."

Before running anything, you should first verify that the file actually exists.

```
ls
```

Never assume a file is present.

---

# Tool Evolution

## Level 1 – Basic Listing

```
ls
```

Lists visible files and directories.

---

## Level 2 – Detailed Listing

```
ls -l
```

Sample Output

```
-rw-r--r-- 1 ec2-user ec2-user 2048 app.log
drwxr-xr-x 2 ec2-user ec2-user 4096 scripts
```

Now you can see:

- Permissions
- Owner
- Group
- File size
- Last modified date

---

## Level 3 – Show Hidden Files

```
ls -a
```

Sample Output

```
.
..
.bashrc
.git
.profile
scripts
```

Many important Linux configuration files begin with a dot (`.`).

For example:

- `.bashrc`
- `.profile`
- `.ssh`

Without `-a`, you won't see them.

---

## Level 4 – Human Readable Sizes

```
ls -lh
```

Instead of:

```
2097152
```

You'll see:

```
2.0M
```

Much easier to read.

---

## Level 5 – My Favorite Combination

```
ls -lah
```

This is one of the most commonly used combinations among Linux administrators because it shows:

- Hidden files
- Permissions
- Ownership
- Human-readable file sizes

If you remember only one variation of `ls`, make it this one.

---

## AWS/DevOps Scenario

You've SSH'd into an EC2 instance.

The deployment guide says:

> "Update the configuration inside the application's directory."

Before opening any file, you first ask:

> **What files are actually present?**

```
ls -lah
```

You notice:

```
.env
docker-compose.yml
nginx.conf
logs/
```

Now you know where to investigate next.

---

## Think Like an Engineer

Don't rush to edit files.

First understand what exists.

Many production mistakes happen because someone edited the wrong file simply because they assumed it was there.

---

## Interview Corner

**Interviewer**

You connect to an unfamiliar server.

How would you quickly inspect the current directory, including hidden files and permissions?

**Expected Discussion**

A strong candidate might answer:

```
ls -lah
```

Then explain:

- `-l` → Long listing
- `-a` → Include hidden files
- `-h` → Human-readable sizes

Interviewers appreciate candidates who explain **why** they chose a command.

---

## Common Mistakes

❌ Using `ls` and assuming no hidden files exist.

❌ Ignoring file permissions.

❌ Editing a file without confirming you're in the correct directory.

---

## Engineering Habit

> **Before modifying anything, inspect the directory.**

---

## Mission Reflection

You now know:

- Which operating system you're using.
- Which user you're logged in as.
- Which server you're connected to.
- Where you are.
- What files are available.

You're beginning to build a mental map of the server before making changes.

That's exactly what experienced engineers do.




# Scene 6 – Is There Enough Disk Space?

You have now identified:

- The operating system
- The current user
- The server
- Your current location
- The files available

Before deploying an application or copying large files, there's one more important question:

> **Does the server have enough free disk space?**

A surprising number of production issues happen because a disk becomes full.

---

## Question 6

### How do I check disk usage?

## Tool – `df`

### Purpose

Display the available and used disk space on mounted file systems.

### Syntax

```
df [options]
```

### AWS Example

```
df -h
```

### Sample Output

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       20G   11G    8G   58% /
tmpfs           975M     0  975M    0% /dev/shm
```

The `-h` option displays sizes in a human-readable format (KB, MB, GB).

---

## Understanding the Output

|Column|Meaning|
|---|---|
|Filesystem|Storage device or partition|
|Size|Total capacity|
|Used|Space already consumed|
|Avail|Free space remaining|
|Use%|Percentage of disk used|
|Mounted on|Mount point|

Among these, the **Use%** column deserves your immediate attention.

---

## Think Like an Engineer

Suppose your application suddenly stops writing logs.

Your first instinct shouldn't be to restart the application.

Instead, ask:

> **Is the disk full?**

One command can quickly answer that question.

```
df -h
```

Sometimes, a full filesystem is the root cause of multiple application failures.

---

## Tool Evolution

### Human-readable output

```
df -h
```

---

### Display a specific filesystem

```
df -h /
```

Useful when checking only the root filesystem.

---

### Display filesystem type

```
df -Th
```

Sample Output

```
Filesystem     Type  Size Used Avail Use% Mounted on
/dev/xvda1     xfs    20G  11G    8G  58% /
```

This helps identify whether the filesystem is `xfs`, `ext4`, or another type.

---

## AWS/DevOps Scenario

You receive an alert:

> **Deployment failed: No space left on device**

Before investigating the application, you SSH into the EC2 instance and run:

```
df -h
```

Output:

```
Filesystem      Size Used Avail Use%
/dev/xvda1       20G 20G     0 100%
```

Now you know the deployment didn't fail because of the application.

It failed because the server has exhausted its storage.

The next step would be identifying what is consuming the disk space.

We'll learn that in a later mission using the `du` command.

---

## Interview Corner

**Interviewer**

A production application suddenly fails to create log files.

What Linux command would you use first?

**Expected Discussion**

A strong candidate might answer:

> "I'd first check the available disk space using `df -h`. If the filesystem is full, that could explain why the application cannot write new files."

Notice the focus on **forming a hypothesis before taking action**.

---

## Common Mistakes

❌ Looking only at the **Size** column.

❌ Ignoring the **Use%** column.

❌ Restarting services before checking whether the disk is full.

---

## Engineering Habit

> **Always verify system resources before troubleshooting the application.**

---

## Mission Reflection

You have now completed the initial health check of an unfamiliar Linux server.

Without changing anything, you discovered:

- Which operating system is running.
- Which user you're logged in as.
- Which server you're connected to.
- Where you are in the filesystem.
- What files are available.
- Whether the server has sufficient storage.

This is exactly how experienced engineers begin working on a new system.


