# Day 5 — Bash Scripting for Automation

Modern infrastructure runs on automation.

Whether you're a:

* DevOps Engineer
* Linux Administrator
* Cloud Engineer
* Cybersecurity Professional
* Backend Developer

You will eventually write Bash scripts.

From automating backups to deploying servers, monitoring systems, managing logs, and running CI/CD pipelines — Bash is everywhere.

If Linux is the operating system of the internet, then Bash is its automation language.

## 🔗 Resources

* **GitHub Repo:**
  [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)

* **Command Sheet:**
  [https://bash-command-sheets-k51c.vercel.app/](https://bash-command-sheets-k51c.vercel.app/)

* **Linux Cron Job Scheduler:**
  [https://crontab.guru/](https://crontab.guru/)

---

many beginners get confused between:

* Bash
* SH
* ZSH
* Fish
* Dash
* Yum
* Apt
* DNF

So before learning Bash scripting, let’s first understand the Linux shell ecosystem.

---

## 🧠 Understanding Linux Shells & Package Managers

Linux has multiple shells.

A shell is simply a command interpreter that lets users communicate with the operating system.

Think of it like:

```text
User → Shell → Linux Kernel → Hardware
```

---

## 🐚 Popular Linux Shells

![bash Scripting](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/q7fdcpns140aretbvo4u.png)


---

## 📦 Linux Package Managers

Package managers install software.

Different Linux distributions use different package managers.

| Distribution | Package Manager | Example              |
| ------------ | --------------- | -------------------- |
| Ubuntu       | APT             | `apt install nginx`  |
| Debian       | APT             | `apt update`         |
| CentOS 7     | YUM             | `yum install docker` |
| RHEL         | YUM/DNF         | `dnf install git`    |
| Fedora       | DNF             | `dnf update`         |
| Arch Linux   | Pacman          | `pacman -S nginx`    |
| Alpine Linux | APK             | `apk add curl`       |


---

## 🚀 What is Bash?

Bash stands for:

> **Bourne Again SHell**

It is the default shell for most Linux distributions.

A shell is simply a program that allows users to interact with the operating system.

Example:

```bash
pwd
ls
mkdir project
```

These commands are executed through the shell.

---

## 🧠 What is Bash Scripting?

A Bash script is a file containing Linux commands executed sequentially.

Instead of manually typing commands repeatedly, you automate them inside a script.

Example:

```bash
#!/bin/bash

echo "Hello Rahul"
date
uptime
```

Save as:

```bash
hello.sh
```

Make executable:

```bash
chmod +x hello.sh
```

Run:

```bash
./hello.sh
```

---

## ⚡ Why Bash Automation Matters

Without automation:

❌ Manual server setup
❌ Repetitive deployments
❌ Manual backups
❌ Human errors
❌ Slow operations

With Bash automation:

✅ Faster workflows
✅ Repeatable processes
✅ Infrastructure consistency
✅ Reduced human mistakes
✅ Better productivity

---

## 📂 Bash Script Structure

Basic structure:

```bash
#!/bin/bash

# Comments start with #

echo "Starting Script"

# Commands here
```

---

### 🔥 Variables in Bash

Variables store data.

---

### ✅ Creating Variables

```bash
name="Rahul"
age=22
city="Delhi"
```

Access variables using `$`.

```bash
echo $name
echo $age
```

---

## ⚠️ Important Rule

No spaces around `=`.

❌ Wrong:

```bash
name = "Rahul"
```

✅ Correct:

```bash
name="Rahul"
```

---

### 🧾 User Input

Take input dynamically.

```bash
#!/bin/bash

echo "Enter your name:"
read name

echo "Welcome $name"
```

---

### 📌 Command Line Arguments

Arguments passed while running scripts.

```bash
#!/bin/bash

echo "First argument: $1"
echo "Second argument: $2"
```

Run:

```bash
./script.sh Rahul Linux
```

Output:

```bash
First argument: Rahul
Second argument: Linux
```

---

### 🔍 Conditional Statements

Conditions allow decision-making.

---

### ✅ If Statement

```bash
#!/bin/bash

age=20

if [ $age -ge 18 ]
then
    echo "Adult"
fi
```

---

### ✅ If-Else

```bash
#!/bin/bash

num=5

if [ $num -gt 10 ]
then
    echo "Greater than 10"
else
    echo "Less than or equal to 10"
fi
```

---

### ✅ If-Elif-Else

```bash
#!/bin/bash

marks=75

if [ $marks -ge 90 ]
then
    echo "Grade A"
elif [ $marks -ge 70 ]
then
    echo "Grade B"
else
    echo "Grade C"
fi
```

---

### 🧠 Comparison Operators

| Operator | Meaning          |
| -------- | ---------------- |
| `-eq`    | Equal            |
| `-ne`    | Not equal        |
| `-gt`    | Greater than     |
| `-lt`    | Less than        |
| `-ge`    | Greater or equal |
| `-le`    | Less or equal    |

---

## 🔁 Loops in Bash

Loops repeat tasks automatically.

---

### ✅ For Loop

```bash
#!/bin/bash

for i in 1 2 3 4 5
do
    echo "Number: $i"
done
```

---

### ✅ Range Loop

```bash
for i in {1..10}
do
    echo $i
done
```

---

### ✅ While Loop

```bash
#!/bin/bash

count=1

while [ $count -le 5 ]
do
    echo $count
    ((count++))
done
```

---

### ✅ Infinite Loop

```bash
while true
do
    echo "Running..."
    sleep 2
done
```

---

### 🔨 Functions in Bash

Functions help organize reusable code.

```bash
#!/bin/bash

greet() {
    echo "Hello $1"
}

greet Rahul
```

---

### 📦 Arrays in Bash

```bash
#!/bin/bash

fruits=("apple" "banana" "mango")

echo ${fruits[0]}
echo ${fruits[1]}
```

Loop through array:

```bash
for fruit in "${fruits[@]}"
do
    echo $fruit
done
```

---

## 📁 File Operations

---

## ✅ Check if File Exists

```bash
#!/bin/bash

if [ -f test.txt ]
then
    echo "File exists"
else
    echo "File not found"
fi
```

---

## ✅ Create File

```bash
touch file.txt
```

---

## ✅ Append to File

```bash
echo "New Log Entry" >> logs.txt
```

---

## ⚙️ Process Automation

---

### ✅ Kill Process

```bash
pkill nginx
```

---

### ✅ Restart Service

```bash
systemctl restart nginx
```

---

### ✅ Check Service Status

```bash
systemctl status docker
```

---

## 🕒 Cron Jobs for Scheduling

Cron automates scripts at scheduled times.

Open cron:

```bash
crontab -e
```



---

### ✅ Run Every Day at Midnight

```bash
0 0 * * * /home/ubuntu/backup.sh
```

---

### ✅ Run Every 5 Minutes

```bash
*/5 * * * * /home/ubuntu/monitor.sh
```

---

## 🚀 Real-World Automation Scripts

Now the fun begins.

---

### 🔥 1️⃣ Automated Backup Script

```bash
#!/bin/bash

SOURCE="/home/ubuntu/data"
DEST="/backup"

DATE=$(date +%Y-%m-%d)

tar -czf $DEST/backup-$DATE.tar.gz $SOURCE

echo "Backup completed"
```

What it does:

✅ Compresses files
✅ Creates timestamp backup
✅ Automates backup process

---

### 🔥 2️⃣ Disk Usage Monitoring Script

```bash
#!/bin/bash

THRESHOLD=80

USAGE=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')

if [ $USAGE -gt $THRESHOLD ]
then
    echo "Disk usage exceeded threshold!"
else
    echo "Disk usage normal"
fi
```

Useful for:

* Servers
* Cloud VMs
* Production systems

---

### 🔥 3️⃣ Website Monitoring Script

```bash
#!/bin/bash

URL="https://example.com"

STATUS=$(curl -o /dev/null -s -w "%{http_code}" $URL)

if [ $STATUS -eq 200 ]
then
    echo "Website is UP"
else
    echo "Website is DOWN"
fi
```

---

### 🔥 4️⃣ Auto Deployment Script

```bash
#!/bin/bash

echo "Pulling latest code..."

git pull origin main

echo "Installing dependencies..."

npm install

echo "Restarting application..."

pm2 restart app
```

Used heavily in:

* DevOps
* CI/CD
* Production deployments

---

### 🔥 5️⃣ Log Cleanup Script

```bash
#!/bin/bash

find /var/log -type f -name "*.log" -mtime +7 -delete

echo "Old logs deleted"
```

Deletes logs older than 7 days.

---

### 🛡️ Error Handling in Bash

Always validate failures.

```bash
#!/bin/bash

mkdir test

if [ $? -eq 0 ]
then
    echo "Directory created"
else
    echo "Failed"
fi
```

`$?` stores previous command status.

* `0` = success
* Non-zero = failure

---

## 📌 Exit Codes

```bash
exit 0
```

Success.

```bash
exit 1
```

Failure.

---

## 🧪 Debugging Bash Scripts

---

### ✅ Run in Debug Mode

```bash
bash -x script.sh
```

Shows command execution step-by-step.

---

### ✅ Strict Mode (Highly Recommended)

```bash
#!/bin/bash

set -euo pipefail
```

This helps catch:

* Undefined variables
* Failed commands
* Pipeline failures

---

## ⚡ Bash Automation in DevOps

Bash is heavily used in:

| Area       | Usage                 |
| ---------- | --------------------- |
| CI/CD      | Build automation      |
| Kubernetes | Cluster scripts       |
| Docker     | Container automation  |
| AWS        | EC2 setup scripts     |
| Monitoring | Health checks         |
| Security   | Log scanning          |
| Linux      | System administration |

---

## 🔐 Security Best Practices

Never write insecure scripts.

---

### ❌ Avoid Hardcoding Passwords

Bad:

```bash
password="admin123"
```

Better:

```bash
read -s password
```

---

### ✅ Quote Variables

Bad:

```bash
rm -rf $dir
```

Good:

```bash
rm -rf "$dir"
```

---

### ✅ Validate Inputs

Always sanitize user input.

---

## 📚 Important Bash Commands Every Engineer Should Know

| Command     | Purpose          |
| ----------- | ---------------- |
| `grep`      | Search text      |
| `awk`       | Text processing  |
| `sed`       | Stream editing   |
| `cut`       | Extract columns  |
| `find`      | Search files     |
| `xargs`     | Command chaining |
| `curl`      | API requests     |
| `tar`       | Archive files    |
| `cron`      | Scheduling       |
| `systemctl` | Manage services  |

---

![Bash Flow](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fog2jfx865yrn8mbmwoz.png)

---

## 🚀 Bash vs Python for Automation

| Bash                      | Python                 |
| ------------------------- | ---------------------- |
| Best for Linux automation | Best for complex logic |
| Fast scripting            | Better readability     |
| Native shell access       | Huge libraries         |
| Lightweight               | Cross-platform         |

Most DevOps engineers use both.

---

## 🎯 Best Practices for Bash Scripting

✅ Use meaningful variable names
✅ Add comments
✅ Use functions
✅ Handle errors properly
✅ Use strict mode
✅ Keep scripts modular
✅ Log important actions
✅ Test before production

---

## 🧠 Final Thoughts

Bash scripting is one of the most valuable skills in Linux, DevOps, Cloud, and Cybersecurity.

The engineers who automate repetitive tasks become exponentially more productive.

Start small:

* Automate backups
* Monitor servers
* Deploy applications
* Clean logs
* Schedule tasks

Over time, Bash becomes your operational superpower.