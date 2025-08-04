# 🚀 Dual GitHub Account Setup (Personal and Organization) with SSH Access in RStudio Projects

## 🎯 Purpose
This SOP describes how to configure Git, RStudio, and SSH to work with both a personal and an organization GitHub account. It is especially useful in collaborative research projects using Git for version control in RStudio.

---

## 🧰 Step 1: Install and Configure Git

### 🔍 1.1 Check if Git is installed

▶️ **Terminal**:
```bash
git --version
```

If not installed, download from: https://git-scm.com/downloads

### 🧾 1.2 Set global Git identity (personal)

▶️ **Terminal**:
```bash
git config --global user.name "your-github-username"
git config --global user.email "your-email@example.com"
```

---

## 🧩 Step 2: Configure RStudio to Use Git

Open RStudio → `Tools > Global Options > Git/SVN`

Ensure the Git executable is detected. If not, manually set its path (e.g., `/usr/bin/git` or `C:\Program Files\Git\bin\git.exe`).

---

## 🔐 Step 3: Create and Register SSH Keys

### 🔑 3.1 Generate SSH keys

▶️ **Terminal** (organization account):
```bash
ssh-keygen -t ed25519 -C "your-org-email@example.com" -f ~/.ssh/id_ed25519_org
```

▶️ **Terminal** (personal account, if not already created):
```bash
ssh-keygen -t ed25519 -C "your-email@example.com" -f ~/.ssh/id_ed25519_personal
```

### 🧠 3.2 Add SSH keys to the agent

▶️ **Terminal**:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_personal
ssh-add ~/.ssh/id_ed25519_org
```

### 🗂️ 3.3 Create SSH config file

▶️ **Terminal**:
```bash
touch ~/.ssh/config
nano ~/.ssh/config
```

Paste the following into the editor:
```
# Personal GitHub account
Host github.com-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal
    IdentitiesOnly yes

# Organization GitHub account
Host github.com-org
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_org
    IdentitiesOnly yes
```

Save with Ctrl + X → Y → Enter.

### ☁️ 3.4 Upload public keys to GitHub

No terminal needed. Go to GitHub → `Settings → SSH and GPG keys → New SSH key`.

To view your key:

▶️ **Terminal**:
```bash
cat ~/.ssh/id_ed25519_org.pub
```

Repeat for the personal key.

---

## 🧱 Step 4: Initialize Git in RStudio Project

In RStudio:

- Go to `Tools > Version Control > Project Setup > Git`
- Optional: generate `.gitignore` in `Tools > Global Options > Git/SVN > Create .gitignore`

Or manually create one with entries like:
```
.Rhistory
.RData
.Rproj.user/
.DS_Store
```

---

## 🏗️ Step 5: Create and Connect to GitHub Organization Repository

On GitHub (organization account), create a new repo **without** README.

▶️ **Terminal** (in your local project folder):
```bash
git remote add origin git@github.com-org:LJohnsonLab/RepoName.git
```

---

## 🔍 Step 6: Confirm Remote Setup

▶️ **Terminal**:
```bash
git remote -v
```

Expected:
```
origin  git@github.com-org:LJohnsonLab/RepoName.git (fetch)
origin  git@github.com-org:LJohnsonLab/RepoName.git (push)
```

---

## 🧪 Step 7: Test SSH Connection

▶️ **Terminal**:
```bash
ssh -T git@github.com-org
```

Expected:
```
Hi LJohnsonLab! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 🚚 Step 8: Push Local Project

▶️ **Terminal**:
```bash
git add .
git commit -m "Initial commit"
git push -u origin main
```

---

## 📝 Final Notes

- Use `git@github.com-org:...` for organization projects and `git@github.com-personal:...` for personal.
- To clone an org repo:
  ```bash
  git clone git@github.com-org:OrgName/RepoName.git
  ```
- If switching machines, re-add SSH keys to the agent and copy your `~/.ssh/config`.
