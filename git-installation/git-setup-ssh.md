## Check SSH Key Configuration

1. **Verify SSH Key Exists**:
   Check if you have an SSH key generated on your machine. Open a terminal and run:
   ```bash
   ls -al ~/.ssh
   ```
   Look for files named `id_rsa` and `id_rsa.pub` (or similar).

2. **Generate a New SSH Key (if needed)**:
   If no keys are present, generate a new SSH key pair:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```
   Press Enter to accept the default file location and enter a passphrase if desired.

3. **Add SSH Key to SSH Agent**:
   Start the SSH agent in the background:
   ```bash
   eval "$(ssh-agent -s)"
   ```
   Then add your SSH key:
   ```bash
   ssh-add ~/.ssh/id_rsa
   ```

4. **Add Public Key to GitHub/GitLab**:
   Copy your public key to the clipboard:
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```
   Go to your GitHub or GitLab account settings and add the public key under **SSH and GPG keys**.

## Test Your Connection

1. **Test SSH Connection**:
   Run the following command to test if your SSH key is correctly set up:
   ```bash
   ssh -T git@github.com
   ```
   or for GitLab:
   ```bash
   ssh -T git@gitlab.com
   ```
   You should see a message confirming successful authentication.

## Ensure Correct Repository URL

1. **Check Remote URL**:
   Verify that the remote URL of your repository is correct. You can check this with:
   ```bash
   git remote -v
   ```
   If it’s incorrect, update it using:
   ```bash
   git remote set-url origin git@github.com:username/repository.git
   ```

## Additional Troubleshooting

- **Permissions Issue**: Ensure that your `.ssh` directory and files have the correct permissions. The directory should be `700` and files should be `600`:
  ```bash
  chmod 700 ~/.ssh
  chmod 600 ~/.ssh/id_rsa
  chmod 644 ~/.ssh/id_rsa.pub
  ```

- **Firewall or Network Issues**: Sometimes, network settings or firewalls can block SSH connections. Ensure that your network allows outbound connections on port 22.

By following these steps, you should be able to resolve the "Permission denied (publickey)" error and successfully connect to your Git repository.

---
To copy your SSH public key to the clipboard so you can easily paste it elsewhere, you can use the following command depending on your operating system:
For macOS

bash
cat ~/.ssh/id_rsa.pub | pbcopy

For Linux
You might need to install xclip or xsel first. Here’s how to do it with xclip:

    Install xclip if it's not already installed:

bash
sudo apt install xclip  # For Debian/Ubuntu
sudo yum install xclip  # For CentOS/RHEL

Then run:

    bash
    cat ~/.ssh/id_rsa.pub | xclip -selection clipboard

For Windows (using Git Bash)
In Git Bash, you can use:

bash
cat ~/.ssh/id_rsa.pub | clip

After running the appropriate command for your OS, your public key will be copied to the clipboard, and you can paste it wherever needed using Ctrl + V or right-clicking and selecting paste.