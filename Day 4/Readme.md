# Day 4 —  Git & GitHub Fundamentals

Modern software development is impossible without version control. Whether you're building applications alone, working in a startup, contributing to open source, or managing enterprise infrastructure — Git and GitHub are at the center of everything.

From tracking code changes to handling production deployments, Git powers modern engineering workflows.

In this guide, we’ll deeply cover:

* What Git actually is
* How Git works internally
* Git basics
* Branching strategies
* Pull Requests
* Merge conflicts
* Git workflows
* Essential Git commands
* Best practices used by professional teams

---

## 🔗 Resources

* **GitHub Repo:**
  [https://github.com/17J/30-Days-Cloud-DevSecOps-Journey](https://github.com/17J/30-Days-Cloud-DevSecOps-Journey)

* **Command Sheet:**
  [https://git-command-sheet-sigma.vercel.app/](https://git-command-sheet-sigma.vercel.app/)

---

## 🌍 What is Git?

Git is a **distributed version control system (DVCS)** created by Linus Torvalds in 2005.

Git helps developers:

* Track changes in code
* Collaborate safely
* Maintain project history
* Revert mistakes
* Manage releases
* Work on multiple features simultaneously

---

## 🧠 Why Git Was Created

Before Git, developers mainly used centralized systems like:

* SVN
* CVS

These systems had problems:

* Slow performance
* Central server dependency
* Difficult branching
* Risk of losing history

Git solved these issues by making every developer’s machine a **complete copy of the repository**.

That means:

* Faster operations
* Offline work
* Safer collaboration
* Better branching support

---

## 🔥 What Makes Git Powerful?

| Feature                  | Why It Matters                              |
| ------------------------ | ------------------------------------------- |
| Distributed architecture | Every developer has full repository history |
| Fast branching           | Lightweight feature development             |
| Snapshots                | Tracks repository states efficiently        |
| Data integrity           | Uses SHA hashing                            |
| Collaboration            | Multiple developers work safely             |

---

## 🌐 What is GitHub?

GitHub is a cloud-based platform that hosts Git repositories.

GitHub adds:

* Remote repository hosting
* Collaboration tools
* Pull Requests
* CI/CD integration
* Code review systems
* Security scanning
* Issue tracking
* Open-source collaboration

Think of it like this:

| Tool   | Purpose                |
| ------ | ---------------------- |
| Git    | Version control engine |
| GitHub | Collaboration platform |

---

## ⚙️ Understanding How Git Works Internally

Many beginners struggle here, but once you understand Git’s internal workflow, everything becomes easier.

Git has **three main areas**:

---

## 1️⃣ Working Directory

This is your actual project folder.

Example:

```bash
app.js
package.json
README.md
```

You edit files here.

---

## 2️⃣ Staging Area (Index)

This is Git’s preparation area.

You choose which changes should go into the next commit.

Example:

```bash
git add app.js
```

Now `app.js` is staged.

---

## 3️⃣ Repository (.git)

This stores:

* Commit history
* Branches
* Metadata
* References

This is Git’s database.

---

## 📦 Git Repository Structure

When you initialize Git:

```bash
git init
```

Git creates:

```bash
.git/
```

Inside `.git`:

* Objects
* References
* Commit history
* Configuration
* Branch metadata

⚠️ Never manually modify `.git`.

---

## 🚀 Git Basics

### 📁 Initializing a Repository

```bash
git init
```

This turns your project into a Git repository.

---

### 📥 Cloning a Repository

```bash
git clone https://github.com/user/project.git
```

This downloads:

* Files
* Commit history
* Branches
* Remote configuration

---

### 📊 Checking Repository Status

```bash
git status
```

Shows:

* Modified files
* Staged changes
* Untracked files

This command becomes your best friend.

---

### ➕ Staging Changes

Stage everything:

```bash
git add .
```

Stage a single file:

```bash
git add server.js
```

---

### 💾 Creating Commits

```bash
git commit -m "Added authentication middleware"
```

A commit is:

* A snapshot
* A checkpoint
* A historical record

---

### 🧾 Good Commit Messages

❌ Bad:

```bash
git commit -m "fix"
```

✅ Good:

```bash
git commit -m "Fixed JWT token expiration handling"
```

Professional teams use:

* Clear descriptions
* Small commits
* Atomic changes

---

## 🌿 Understanding Branches

Branches are one of Git’s biggest strengths.

A branch is simply a movable pointer to commits.

---

## 🏗️ Why Branches Matter

Without branches:

* Everyone edits the same code
* Features collide
* Production becomes unstable

Branches solve this.

---

## 🔹 Creating Branches

```bash
git branch feature-login
```

Switch to branch:

```bash
git checkout feature-login
```

Modern shortcut:

```bash
git checkout -b feature-login
```

---

## 🔍 Viewing Branches

```bash
git branch
```

Current branch shows:

```bash
* main
```

---

## 🔀 Merging Branches

Merge feature into main:

```bash
git checkout main
git merge feature-login
```

Git combines histories.

---

## ⚠️ Merge Conflicts

Conflicts happen when:

* Two developers edit the same lines
* Git cannot automatically decide

Example conflict:

```bash
<<<<<<< HEAD
console.log("Old Code");
=======
console.log("New Code");
>>>>>>> feature-login
```

You must manually resolve it.

---

## 🌳 Branching Strategies

### 1️⃣ Feature Branch Workflow

Each feature gets a separate branch.

Example:

```bash
feature-payment
feature-auth
feature-dashboard
```

### Flow

1. Create branch
2. Develop feature
3. Open Pull Request
4. Review
5. Merge

### Advantages

* Cleaner history
* Safer development
* Easy rollback

Most startups use this.

---

### 2️⃣ Git Flow

One of the most famous workflows.

Popularized by **Vincent Driessen**.

---

## Git Flow Branches

![Git Flow](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/obiumki6ipr2jmdi1h6c.png)

| Branch      | Purpose             |
| ----------- | ------------------- |
| `main`      | Production-ready    |
| `develop`   | Integration branch  |
| `feature/*` | New features        |
| `release/*` | Release preparation |
| `hotfix/*`  | Emergency fixes     |

---

## Git Flow Lifecycle

### 🔹 Feature Development

```bash
git checkout develop
git checkout -b feature-auth
```

---

### 🔹 Release Creation

```bash
git checkout -b release-v1.0
```

---

### 🔹 Hotfix Production Bugs

```bash
git checkout main
git checkout -b hotfix-login
```

---

## ✅ Advantages

* Organized releases
* Stable production
* Enterprise friendly

---

## ❌ Disadvantages

* Complex workflow
* Too many branches
* Slower deployments

---

### 3️⃣ Trunk-Based Development

Used heavily in:

* DevOps teams
* CI/CD environments
* Cloud-native engineering

Core idea:

* Small commits
* Frequent merges
* Short-lived branches

Main branch = trunk.

---

## ✅ Benefits

* Faster deployments
* Smaller conflicts
* Continuous integration friendly

Popular in:

* Google
* Netflix
* Meta

---

## 🔄 Pull Requests (PRs)

A Pull Request is a proposal to merge code changes.

PRs are central to modern collaboration.

---

## 🧩 Pull Request Lifecycle

## Step 1 — Create Branch

```bash
git checkout -b feature-auth
```

---

## Step 2 — Push Branch

```bash
git push origin feature-auth
```

---

## Step 3 — Open PR on GitHub

GitHub compares:

* Source branch
* Target branch

---

## Step 4 — Code Review

Teams review:

* Logic
* Security
* Readability
* Performance

---

## Step 5 — CI/CD Runs

Automated checks:

* Tests
* Linting
* Security scans

---

## Step 6 — Merge PR

Once approved:

* Squash merge
* Rebase merge
* Standard merge

---

## 🔐 Why Pull Requests Matter

PRs improve:

* Code quality
* Security
* Team collaboration
* Knowledge sharing

They act like a checkpoint before production.

---

## 🧠 Types of Git Merges

### 🔹 Fast Forward Merge

Simple linear merge.

Occurs when no divergence exists.

---

### 🔹 Three-Way Merge

Git creates a merge commit.

Most common in teams.

---

### 🔹 Squash Merge

Combines all commits into one.

Creates cleaner history.

---

### 🔹 Rebase

Moves commits on top of the latest branch.

Example:

```bash
git rebase main
```

### Benefits

* Linear history
* Cleaner logs

### Risk

* Can rewrite history

---

## ⚙️ Common Git Workflows

### 1️⃣ Centralized Workflow

Everyone pushes to the same branch.

Simple but risky.

Best for:

* Small teams
* Learning

---

### 2️⃣ Feature Branch Workflow

Separate branches for features.

Most commonly used workflow today.

---

### 3️⃣ Forking Workflow

Mostly used in open source.

### Flow

1. Fork repository
2. Clone fork
3. Create branch
4. Push changes
5. Open PR to original repo

Used heavily on GitHub.

---

## 🚨 Common Git Problems

### ❌ Detached HEAD State

Occurs when checking out commits directly.

Example:

```bash
git checkout 93f4c2
```

You are no longer on a branch.

---

### ❌ Force Push Problems

```bash
git push --force
```

Dangerous because:

* Rewrites history
* Can delete teammates’ work

Use carefully.

---

### ❌ Huge Commits

Huge commits are bad because:

* Hard to review
* Hard to debug
* Hard to revert

---

## 🧹 Git Best Practices

### ✅ Commit Frequently

Small commits = easier debugging.

---

### ✅ Use Meaningful Branch Names

✅ Good:

```bash
feature-payment-api
bugfix-auth-timeout
```

❌ Bad:

```bash
test123
newbranch
```

---

### ✅ Protect Main Branch

Never directly push production-breaking code.

---

### ✅ Pull Before Push

```bash
git pull origin main
```

Avoids conflicts.

---

### ✅ Use `.gitignore`

Prevent sensitive or unnecessary files.

Example:

```bash
node_modules/
.env
dist/
coverage/
```

---

## 🔐 Git in DevOps & CI/CD

Git is deeply integrated into:

* CI/CD pipelines
* Infrastructure as Code
* Kubernetes deployments
* DevSecOps automation

Modern pipelines trigger automatically when:

* Code is pushed
* PRs are merged
* Releases are tagged

Popular integrations:

* GitHub Actions
* GitLab CI/CD
* Jenkins
* CircleCI

---

## 🚀 Final Thoughts

Git is more than just commands.

It’s a system that enables:

* Safe collaboration
* Scalable development
* Reliable deployments
* Modern DevOps practices

Once you truly understand:

* Branching
* Commits
* PRs
* Workflows
* Collaboration strategies

…you stop being just a coder and start working like a professional software engineer.

The best way to master Git is:

* Use it daily
* Break things
* Resolve conflicts
* Work on real projects

Because every professional developer spends a huge part of their career inside Git.
