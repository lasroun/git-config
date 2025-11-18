# Git Configuration & SSH Key Setup Guide

Complete guide for setting up Git with SSH authentication for GitHub, GitLab, and other Git platforms.

## Table of Contents

- [Git Configuration](#git-configuration)
- [SSH Key Generation](#ssh-key-generation)
- [Adding SSH Key to GitHub/GitLab](#adding-ssh-key-to-githubgitlab)
- [Testing SSH Connection](#testing-ssh-connection)
- [Common Git Commands](#common-git-commands)
- [Troubleshooting](#troubleshooting)

## Prerequisites

- Git installed on your system
- Access to GitHub/GitLab account

## Git Configuration

### Step 1: Check Git Installation

```bash
git --version
```

### Step 2: Configure Git User Information

Set your username and email (this will appear in your commits):

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Step 3: Verify Configuration

```bash
git config --list
```

Or check specific values:

```bash
git config user.name
git config user.email
```

### Step 4: Additional Useful Configurations

**Set default branch name to 'main':**

```bash
git config --global init.defaultBranch main
```

**Enable colored output:**

```bash
git config --global color.ui auto
```

**Set default text editor:**

```bash
# For nano
git config --global core.editor nano

# For vim
git config --global core.editor vim

# For VS Code
git config --global core.editor "code --wait"
```

**Configure line endings:**

```bash
# For Linux/Mac
git config --global core.autocrlf input

# For Windows
git config --global core.autocrlf true
```

## SSH Key Generation

SSH keys allow secure authentication without entering passwords every time.

### Step 1: Check for Existing SSH Keys

```bash
ls -la ~/.ssh
```

Look for files like:

- `id_rsa` and `id_rsa.pub` (RSA keys)
- `id_ed25519` and `id_ed25519.pub` (Ed25519 keys - recommended)

### Step 2: Generate New SSH Key Pair

**Using Ed25519 (recommended - more secure and faster):**

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
```

**Using RSA (if Ed25519 is not supported):**

```bash
ssh-keygen -t rsa -b 4096 -C "your.email@example.com"
```

**During key generation:**

- Press `Enter` to accept default file location (`~/.ssh/id_ed25519`)
- Enter a secure passphrase (optional but recommended)
- Confirm the passphrase

**Output example:**

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/username/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/username/.ssh/id_ed25519
Your public key has been saved in /home/username/.ssh/id_ed25519.pub
```

### Step 3: Start SSH Agent

```bash
eval "$(ssh-agent -s)"
```

### Step 4: Add SSH Key to SSH Agent

**For Ed25519:**

```bash
ssh-add ~/.ssh/id_ed25519
```

**For RSA:**

```bash
ssh-add ~/.ssh/id_rsa
```

### Step 5: Display Your Public Key

**For Ed25519:**

```bash
cat ~/.ssh/id_ed25519.pub
```

**For RSA:**

```bash
cat ~/.ssh/id_rsa.pub
```

Copy the entire output (starts with `ssh-ed25519` or `ssh-rsa`).

## Adding SSH Key to GitHub/GitLab

### GitHub

1. Copy your public key:

   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

2. Go to GitHub:

   - Navigate to **Settings** → **SSH and GPG keys**
   - Or visit: https://github.com/settings/keys

3. Click **"New SSH key"**

4. Fill in the details:
   - **Title**: Give it a descriptive name (e.g., "My Laptop - Ubuntu")
   - **Key**: Paste your public key
   - Click **"Add SSH key"**

### GitLab

1. Copy your public key:

   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

2. Go to GitLab:

   - Navigate to **Preferences** → **SSH Keys**
   - Or visit: https://gitlab.com/-/profile/keys

3. Fill in the details:
   - **Key**: Paste your public key
   - **Title**: Give it a descriptive name
   - **Expiration date**: Optional
   - Click **"Add key"**

### Bitbucket

1. Copy your public key:

   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

2. Go to Bitbucket:

   - Navigate to **Personal settings** → **SSH keys**
   - Or visit: https://bitbucket.org/account/settings/ssh-keys/

3. Click **"Add key"**
   - **Label**: Give it a descriptive name
   - **Key**: Paste your public key
   - Click **"Add key"**

## Testing SSH Connection

### Test GitHub Connection

```bash
ssh -T git@github.com
```

**Expected output:**

```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

### Test GitLab Connection

```bash
ssh -T git@gitlab.com
```

**Expected output:**

```
Welcome to GitLab, @username!
```

### Test Bitbucket Connection

```bash
ssh -T git@bitbucket.org
```

## Common Git Commands

### Initial Repository Setup

**Clone a repository using SSH:**

```bash
git clone git@github.com:username/repository.git
```

**Initialize a new repository:**

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin git@github.com:username/repository.git
git push -u origin main
```

### Daily Workflow

**Check status:**

```bash
git status
```

**Add files:**

```bash
git add filename.txt          # Add specific file
git add .                     # Add all files
```

**Commit changes:**

```bash
git commit -m "Your commit message"
```

**Push to remote:**

```bash
git push                      # Push to current branch
git push origin main          # Push to main branch
```

**Pull from remote:**

```bash
git pull                      # Pull from current branch
git pull origin main          # Pull from main branch
```

**View commit history:**

```bash
git log
git log --oneline             # Compact view
```

### Branch Management

**Create and switch to new branch:**

```bash
git checkout -b feature-branch
```

**Switch branches:**

```bash
git checkout main
```

**List branches:**

```bash
git branch                    # Local branches
git branch -a                 # All branches (local + remote)
```

**Delete branch:**

```bash
git branch -d feature-branch  # Safe delete
git branch -D feature-branch  # Force delete
```

### Remote Management

**View remotes:**

```bash
git remote -v
```

**Change remote URL from HTTPS to SSH:**

```bash
git remote set-url origin git@github.com:username/repository.git
```

## Configuration File Locations

### Global Configuration

Located at: `~/.gitconfig`

View contents:

```bash
cat ~/.gitconfig
```

Example content:

```ini
[user]
    name = Your Name
    email = your.email@example.com
[init]
    defaultBranch = main
[color]
    ui = auto
[core]
    editor = nano
```

### SSH Configuration

Located at: `~/.ssh/config`

Create/edit for multiple accounts:

```bash
nano ~/.ssh/config
```

Example configuration:

```
# GitHub account
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519

# GitLab account
Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/id_ed25519

# Work GitHub account (if you have multiple)
Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
```

## Security Best Practices

### 1. Use Strong Passphrases

Always use a passphrase for your SSH keys:

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
# Enter a strong passphrase when prompted
```

### 2. Set Correct Permissions

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/config
```

### 3. Never Share Private Keys

- **Public key** (`.pub`): Safe to share, add to GitHub/GitLab
- **Private key** (no extension): NEVER share or commit to repositories

### 4. Rotate Keys Regularly

Generate new keys every 1-2 years and update them on all platforms.

### 5. Use Different Keys for Different Services (Optional)

```bash
# Generate separate keys
ssh-keygen -t ed25519 -C "github@email.com" -f ~/.ssh/id_ed25519_github
ssh-keygen -t ed25519 -C "gitlab@email.com" -f ~/.ssh/id_ed25519_gitlab
```

## Troubleshooting

### Permission Denied (publickey)

**Problem:**

```
Permission denied (publickey).
fatal: Could not read from remote repository.
```

**Solutions:**

1. **Verify SSH key is added:**

   ```bash
   ssh-add -l
   ```

2. **Add SSH key if not listed:**

   ```bash
   ssh-add ~/.ssh/id_ed25519
   ```

3. **Check SSH connection:**

   ```bash
   ssh -T git@github.com
   ```

4. **Verify remote URL uses SSH:**

   ```bash
   git remote -v
   # Should show: git@github.com:username/repo.git
   # Not: https://github.com/username/repo.git
   ```

5. **Change HTTPS to SSH:**
   ```bash
   git remote set-url origin git@github.com:username/repository.git
   ```

### SSH Agent Not Running

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### Multiple GitHub/GitLab Accounts

Create `~/.ssh/config`:

```
# Personal GitHub
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal

# Work GitHub
Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
```

Clone using custom host:

```bash
git clone git@github-work:company/repository.git
```

### Bad Permissions Error

```bash
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

### Key Not Accepted

1. **Verify key is on GitHub/GitLab:**

   - Check Settings → SSH Keys

2. **Verify public key matches:**

   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

3. **Regenerate if necessary:**
   ```bash
   ssh-keygen -t ed25519 -C "your.email@example.com"
   ```

## Quick Reference

### Generate SSH Key

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
```

### Display Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

### Configure Git

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Test Connection

```bash
ssh -T git@github.com
```

### Clone Repository

```bash
git clone git@github.com:username/repository.git
```

## Additional Resources

- [GitHub SSH Documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [GitLab SSH Documentation](https://docs.gitlab.com/ee/user/ssh.html)
- [Git Official Documentation](https://git-scm.com/doc)
- [Pro Git Book (Free)](https://git-scm.com/book/en/v2)

## License

This guide is provided as-is for educational purposes.

---

**Last Updated:** November 2025  
**Tested on:** Ubuntu 20.04, 22.04, Debian 11, Fedora 38, macOS, Windows (Git Bash)
