# BOOK_ROADMAP.md

# Linux for AWS & DevOps Interviews — Book Roadmap

## Book Mission

Build Linux confidence through realistic AWS and DevOps situations.

The reader should progress from:

**“I know some Linux commands.”**

to:

> **“I can enter an unfamiliar Linux server, investigate it methodically, troubleshoot it, and explain my reasoning in an interview.”**

---

## Book Progression

### Chapter 1 — Meet Your Server

**Status: ✅ Complete**

Mission 1 — Meet Your Server

Core skills:

- Identify the operating system.
- Understand kernel and architecture information.
- Identify the current user.
- Understand permissions and groups.
- Verify hostname.
- Navigate the current environment.
- Inspect files and directories.
- Check disk space.

Core engineering habit:

> **Observe before acting.**

The chapter is complete in `manuscript/01-Mission-Meet-Your-Server.md`.

The next mission established in the manuscript is:

**Mission 2 — Explore the Linux File System**

---

### Chapter 2 — Explore the Linux File System

**Status: 🔜 Next**

Focus:

- Linux filesystem structure.
- Absolute and relative paths.
- `cd`, `pwd`, `ls`.
- Creating directories and files.
- `mkdir`, `touch`.
- Copying and moving files.
- `cp`, `mv`.
- Safe deletion with `rm` and `rmdir`.
- Understanding important Linux directories.
- Applying filesystem knowledge to AWS servers.

Engineering habit:

> **Know where you are before changing what is there.**

---

### Chapter 3 — Files, Permissions & Ownership

Focus:

- File permissions.
- Users and groups.
- Ownership.
- `chmod`.
- `chown`.
- `chgrp`.
- Numeric and symbolic permissions.
- `sudo`.
- Permission-denied troubleshooting.
- Secure operational practices.

Engineering habit:

> **Check ownership and permissions before escalating privileges.**

---

### Chapter 4 — Processes & Resource Usage

Focus:

- What a Linux process is.
- Process identification.
- `ps`.
- `top`.
- `htop`.
- CPU and memory investigation.
- Process states.
- Background and foreground processes.
- Signals.
- `kill`.
- Troubleshooting a resource-hungry process.

AWS scenario:

An EC2 application is consuming excessive CPU.

Engineering habit:

> **Find the process before killing the process.**

---

### Chapter 5 — Services & System Operations

Focus:

- Services and daemons.
- `systemctl`.
- Service status.
- Starting and stopping services.
- Restarting safely.
- Enable/disable behavior.
- Service failures.
- Basic system startup concepts.

AWS scenario:

An application on an EC2 instance is unreachable because its service is not running.

Engineering habit:

> **Check status before restarting.**

---

### Chapter 6 — Networking from the Linux Server

Focus:

- IP addresses.
- Interfaces.
- Routing.
- DNS.
- Ports.
- Connectivity testing.
- `ip`.
- `ss`.
- `ping`.
- `curl`.
- `dig` / `nslookup`.
- Local versus remote connectivity.

AWS scenario:

An application can reach the server locally but users cannot reach it remotely.

Engineering habit:

> **Separate DNS, routing, port, and application problems.**

---

### Chapter 7 — Logs & Incident Investigation

Focus:

- Why logs matter.
- `/var/log`.
- `journalctl`.
- `tail`.
- `grep`.
- Filtering and searching logs.
- Timestamps.
- Correlating events.
- Finding evidence instead of guessing.

AWS scenario:

A production application started failing after a deployment.

Engineering habit:

> **Let evidence lead the investigation.**

---

### Chapter 8 — Storage, Disk Space & Filesystems

Focus:

- Disk usage.
- Filesystem capacity.
- `df`.
- `du`.
- Mount points.
- Inodes.
- Large-file investigation.
- Full-disk incidents.
- Safe cleanup.

AWS scenario:

An application stops writing data because the EC2 filesystem is full.

Engineering habit:

> **Identify what consumed the space before deleting anything.**

---

### Chapter 9 — Shell Tools for Everyday Troubleshooting

Focus:

- Pipes.
- Redirection.
- `grep`.
- `awk`.
- `sed`.
- `sort`.
- `uniq`.
- `cut`.
- `wc`.
- Command composition.

The goal is not to turn the reader into a shell programmer.

The goal is to make the reader effective at extracting useful information from a Linux system.

Engineering habit:

> **Combine simple tools to answer specific questions.**

---

### Chapter 10 — Environment Variables, Packages & System Configuration

Focus:

- Environment variables.
- `env`.
- `export`.
- Package managers.
- Installing and verifying packages.
- Configuration files.
- PATH.
- Common configuration mistakes.

AWS scenario:

An application behaves differently after deployment because its runtime environment differs from expectations.

Engineering habit:

> **Verify the environment before changing the application.**

---

### Chapter 11 — Linux Troubleshooting Missions

This chapter brings the individual skills together.

Possible investigations:

- High CPU.
- High memory usage.
- Full disk.
- Service unavailable.
- Port not listening.
- DNS failure.
- Permission denied.
- Missing file.
- Application failure.
- Unexpected process.

Each investigation should require the reader to decide:

**What do I know?  
What don't I know?  
What evidence do I need next?**

Engineering habit:

> **Troubleshoot systematically, not randomly.**

---

### Chapter 12 — Linux Interview Missions

The final chapter shifts from learning to interview performance.

Focus:

- “Walk me through your first five minutes on a Linux server.”
- “A server is slow. How do you investigate?”
- “Disk usage is 100%. What do you do?”
- “A service is down. How do you troubleshoot it?”
- “The application works locally but not remotely. Why?”
- “Permission denied — how do you investigate?”
- Scenario-based command selection.
- Explaining trade-offs.
- Thinking aloud during interviews.

The reader should demonstrate a process rather than recite commands.

Engineering habit:

> **Explain your reasoning, not just your commands.**

---

## Book Completion Standard

The book is complete only when every chapter has:

- A coherent engineering story.
- Practical Linux commands.
- AWS/DevOps context.
- Interview relevance.
- Hands-on practice.
- Knowledge checks.
- Troubleshooting reasoning.
- A mission progression.
- Tested commands.
- Editorial review against `STYLE_GUIDE.md`.

---

## Planned Reader Journey

**Meet the server**

↓

**Explore the filesystem**

↓

**Understand permissions**

↓

**Understand processes**

↓

**Manage services**

↓

**Investigate networking**

↓

**Read logs**

↓

**Investigate storage**

↓

**Combine shell tools**

↓

**Understand configuration**

↓

**Solve incidents**

↓

**Demonstrate the skill in interviews**

---

## Next Writing Target

The immediate writing target is:

> **Chapter 2 — Mission 2: Explore the Linux File System**

Do not jump ahead to later chapters until Mission 2 is developed and reviewed.