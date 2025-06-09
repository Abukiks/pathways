## SSH Key Setup & Troubleshooting Guide

### 1. Verify SSH Key Exists

Check if you already have SSH keys on your machine:

```bash
ls -al ~/.ssh
```

Look for files like `id_rsa` and `id_rsa.pub` (or `id_ed25519` and `.pub`).

---

### 2. Generate a New SSH Key (if none found)

If no key exists, generate one:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

* Press Enter to accept the default file location.
* Optionally enter a passphrase for added security.

---

### 3. Add SSH Key to SSH Agent

Start the SSH agent:

```bash
eval "$(ssh-agent -s)"
```

Add your private key to the agent:

```bash
ssh-add ~/.ssh/id_rsa
```

---

### 4. Add Your Public Key to GitHub or GitLab

Copy the public key to your clipboard:

* **macOS:**

  ```bash
  cat ~/.ssh/id_rsa.pub | pbcopy
  ```

* **Linux (with `xclip`):**

  ```bash
  sudo apt install xclip        # Debian/Ubuntu
  sudo yum install xclip        # CentOS/RHEL
  cat ~/.ssh/id_rsa.pub | xclip -selection clipboard
  ```

* **Windows (Git Bash):**

  ```bash
  cat ~/.ssh/id_rsa.pub | clip
  ```

Then go to your **GitHub** or **GitLab** account settings → **SSH and GPG keys**, and paste your key there.

---

### 5. Test SSH Connection

Test if the SSH setup works:

* For GitHub:

  ```bash
  ssh -T git@github.com
  ```

* For GitLab:

  ```bash
  ssh -T git@gitlab.com
  ```

You should see a success message confirming authentication.

---

### 6. Verify Repository Remote URL

Check your remote URL:

```bash
git remote -v
```

If the URL is incorrect or uses HTTPS instead of SSH, update it:

```bash
git remote set-url origin git@github.com:username/repository.git
```

---

### 7. Fix Common Permission Issues

Ensure your `.ssh` directory and files have proper permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
```

---

### 8. Check Network/Firewall Settings

Make sure outbound SSH connections on port 22 are allowed by your firewall or network.

---

## FAQ: Permission Denied Error

**Error:**

```
ERROR: Permission to xJohnlrac/pathways.git denied to jabucay32.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

**Solution:**

This often happens when your Git remote URL points to the wrong SSH host or account. Fix it by updating your remote URL:

```bash
git remote set-url origin git@github-personal:xJohnlrac/pathways.git
```

(Replace `git@github-personal` with the correct SSH host alias configured in your `~/.ssh/config` if applicable.)
