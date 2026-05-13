# Day 2 - Linux Fundamentals


## 🐧 Linux Fundamentals Every DevSecOps & Cloud Engineer Should Know

There’s a funny truth in tech:

You can spend years learning Kubernetes, Docker, Terraform, CI/CD, and cloud platforms…
but sooner or later, everything still comes back to one thing:

**Linux.**

Servers run on Linux.
Containers run on Linux.
Most cloud systems rely on Linux.
Even modern DevSecOps pipelines silently depend on Linux permissions, users, file systems, and shell commands working correctly.

And yet many beginners jump directly into “advanced DevOps” without understanding the basics.

That’s like trying to fly a fighter jet before learning how steering works.

So in this blog, let’s break down Linux Fundamentals in a practical, beginner-friendly, and industry-focused way.



- **Github Repo:** [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)
- **Command Sheet:** [https://bash-command-sheets-k51c.vercel.app/](https://bash-command-sheets-k51c.vercel.app/)


---

## 🚀 Why Linux Matters So Much

If you enter any of these fields:

* DevOps
* Cloud Engineering
* Cybersecurity
* Site Reliability Engineering (SRE)
* Backend Development
* Platform Engineering
* Kubernetes Administration

…Linux becomes unavoidable.

Most production servers worldwide run Linux because it is:

* Stable
* Lightweight
* Secure
* Open-source
* Highly customizable
* Automation friendly

That’s why companies like:

* Google
* Amazon
* Netflix
* Meta

all heavily rely on Linux infrastructure.

---

## 🖥️ Understanding Linux Basics

Linux is an operating system kernel that powers many distributions (distros) like:

* Ubuntu
* Debian
* CentOS
* Fedora
* Arch Linux

Think of Linux as the “engine” and distributions as different car models built around it.

In DevOps, the most commonly used distro is usually:

* Ubuntu Server
* Debian
* RHEL / Rocky Linux

---

### 📂 Linux File System Explained


One of the first things beginners notice:

Linux does NOT use drives like:

```bash
C:\
D:\
```

Instead, Linux starts from a single root directory:

```bash
/
```

Everything exists under this root.

---

![Linux File System ](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fbimj40jdyx25cfr6g03.png)

### 📁 Important Linux Directories

| Directory | Purpose                    |
| --------- | -------------------------- |
| `/`       | Root directory             |
| `/home`   | User files                 |
| `/etc`    | Configuration files        |
| `/var`    | Logs and variable data     |
| `/bin`    | Essential commands         |
| `/tmp`    | Temporary files            |
| `/root`   | Root user home             |
| `/usr`    | Applications and utilities |

Example:

```bash
/home/rahul/projects
```

means:

* `home` directory
* inside it → user `rahul`
* inside it → folder `projects`

---

## ⚡ Essential Linux Commands

### 📍 1. Check Current Directory

```bash
pwd
```

Output:

```bash
/home/ubuntu
```

This tells you where you currently are.

---

### 📍 2. List Files

```bash
ls
```

Detailed list:

```bash
ls -la
```

This shows:

* hidden files
* permissions
* ownership
* timestamps

---

### 📍 3. Change Directory

```bash
cd /var/log
```

Go back:

```bash
cd ..
```

Go to home:

```bash
cd ~
```

---

### 📍 4. Create Files & Folders

Create folder:

```bash
mkdir devops
```

Create file:

```bash
touch notes.txt
```

---

### 📍 5. Copy, Move & Delete

Copy:

```bash
cp file.txt backup.txt
```

Move:

```bash
mv old.txt new.txt
```

Delete:

```bash
rm file.txt
```

Delete folder recursively:

```bash
rm -rf foldername
```

⚠️ Be careful with `rm -rf`.

One wrong command can destroy an entire server.

Yes… this has happened in real companies.

---

## 🔍 Viewing File Content

View file:

```bash
cat file.txt
```

Large files:

```bash
less file.txt
```

Real-time logs:

```bash
tail -f app.log
```

This is extremely common in DevOps troubleshooting.

---

## 🔐 Linux Permissions Explained

This is where Linux becomes VERY important for security.

Run:

```bash
ls -l
```

You may see:

```bash
-rwxr-xr--
```

Looks scary at first.

But it’s simple once broken down.

---


## 🧠 Permission Structure

![Permission Demo](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zje85egw4acoc9pea668.png)
Example:

```bash
-rwxr-xr--
```

Split it:

```bash
rwx | r-x | r--
```

These represent:

| Section | Meaning            |
| ------- | ------------------ |
| First   | Owner permissions  |
| Second  | Group permissions  |
| Third   | Others permissions |

---

## 📌 Permission Types

| Symbol | Meaning |
| ------ | ------- |
| r      | Read    |
| w      | Write   |
| x      | Execute |

---

## 🔧 Changing Permissions

Give execute permission:

```bash
chmod +x script.sh
```

Specific permissions:

```bash
chmod 755 script.sh
```

---

## 🤔 What is 755?

Numbers represent permissions:

| Number | Permission |
| ------ | ---------- |
| 7      | rwx        |
| 5      | r-x        |
| 5      | r-x        |

So:

```bash
755
```

means:

* Owner → full access
* Group → read & execute
* Others → read & execute

This is very common for scripts and applications.

---

### 👤 Users & Groups in Linux

Linux is a multi-user operating system.

That means multiple users can work on the same machine securely.

---

### 👥 Create a User

```bash
sudo adduser rahul
```

Set password and details.

---

### 👥 Create a Group

```bash
sudo groupadd developers
```

---

### 🔗 Add User to Group

```bash
sudo usermod -aG developers rahul
```

This is heavily used in:

* DevOps teams
* Shared servers
* Kubernetes nodes
* CI/CD runners

---

## 👑 The Root User

Linux has a superuser called:

```bash
root
```

Root can do EVERYTHING.

That’s why production systems usually avoid direct root access.

Instead, admins use:

```bash
sudo
```

Example:

```bash
sudo apt update
```

This temporarily gives admin privileges.

---

## 📦 Package Management

Linux installs software using package managers.

Ubuntu:

```bash
sudo apt install nginx
```

CentOS/RHEL:

```bash
sudo yum install nginx
```

or

```bash
sudo dnf install nginx
```

---

## 🌐 Real DevOps Connection

Here’s the reality:

When CI/CD pipelines fail…

You often debug Linux.

When containers crash…

You inspect Linux logs.

When Kubernetes breaks…

You SSH into Linux nodes.

When cloud servers slow down…

You monitor Linux processes.

Linux is not “optional knowledge” anymore.

It is the foundation layer of modern infrastructure.

---

## 🔥 Linux Skills That Make You Valuable

Companies LOVE engineers who can:

✅ Navigate servers confidently
✅ Debug issues quickly
✅ Understand permissions
✅ Manage users securely
✅ Read logs efficiently
✅ Automate shell tasks
✅ Work comfortably in terminal environments

Because these skills directly impact:

* uptime
* security
* deployments
* incident response
* infrastructure stability

---

## Some Examples

### 🔹 Create users & groups

```bash
adduser testuser
```

---

### 🔹 Change permissions

```bash
chmod 700 secret.txt
```

---

### 🔹 Explore logs

```bash
cd /var/log
```

---

### 🔹 Install software

```bash
sudo apt install nginx
```

---

### 🔹 Create shell scripts

```bash
#!/bin/bash
echo "Hello Linux"
```

---

## 💡 Final Thoughts

Most people try to skip Linux fundamentals because they look “basic.”

But experienced engineers know the truth:

The stronger your Linux foundation,
the easier DevOps, Cloud, Security, Docker, and Kubernetes become.

Linux is one of those skills that compounds over time.

At first, commands feel confusing.

Then one day, you realize you’re managing servers, automating deployments, debugging production issues, and writing shell scripts without even thinking.

That’s when Linux stops feeling like an operating system…

…and starts feeling like a superpower. 🚀
