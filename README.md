# VM Setup Guide

This guide provides step-by-step instructions to set up a VM with essential tools for web development. This setup includes installing Nginx, Node.js (LTS), Git, generating SSH keys for Git projects, and setting permissions on `/var/www/html`.

---

## Prerequisites
- Ensure you have SSH access to the VM.
- Run all commands with a user with `sudo` privileges.

---

## 1. Install Nginx
Nginx is a high-performance web server. Install it with the following commands:

```bash
sudo apt update
sudo apt install -y nginx
```

Verify that Nginx is running:

```bash
sudo systemctl status nginx
```

## 2. Install NVM (Node Version Manager)
NVM allows you to manage Node.js versions. Install the latest version of NVM:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

Reload the shell environment to recognize nvm commands:
```bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

## 3. Install Node.js LTS
Use NVM to install the latest LTS version of Node.js:

```bash
nvm install --lts
nvm use --lts
```

Verify the installation:

```bash
node -v
npm -v
```

## 4. Install Git
Git is essential for version control. Install Git with:

```bash
sudo apt install -y git
```

Verify the installation:

```bash
git --version
```

## 5. Generate SSH Keys for Multiple Git Projects
If you need separate SSH keys for multiple Git repositories, follow these steps:

### 5.1 Create SSH Keys
Generate an SSH key for each project, naming them to keep track:

```bash
ssh-keygen -t ed25519 -C "project1@example.com" -f ~/.ssh/project1_key
ssh-keygen -t ed25519 -C "project2@example.com" -f ~/.ssh/project2_key
```
Replace "project1@example.com" and "project2@example.com" with appropriate email addresses.

5.2 Configure SSH for Each Project
Edit (or create) your SSH configuration file:

```bash
nano ~/.ssh/config
```
Add the following entries, specifying each key file and host:
```plaintext
# Project 1
Host project1.github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/project1_key

# Project 2
Host project2.github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/project2_key
```
When cloning or pushing to Git repositories, use project1.github.com or project2.github.com as the host.

### 5.3 Add SSH Keys to SSH Agent
Start the SSH agent and add your keys:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/project1_key
ssh-add ~/.ssh/project2_key
```

## 6. Grant Git Permission to Write to `/var/www/html`
To allow Git operations within `/var/www/html`, adjust ownership or permissions.

### 6.1 Change Directory Ownership
Replace your_user with the username performing Git operations:

```bash
sudo chown -R your_user:www-data /var/www/html
```

### 6.2 Set Directory Permissions

```bash
sudo chmod -R 775 /var/www/html
```

Now, your_user has the necessary permissions to write to `/var/www/html`.

---

## Verification

- Confirm Nginx is running by navigating to your server’s IP in a browser or using curl http://localhost.
- Verify Node.js and npm versions with node -v and npm -v.
- Test Git SSH access with ssh -T git@project1.github.com and ssh -T git@project2.github.com.

--- 

Additional Notes

- For other Git providers (e.g., GitLab, Bitbucket), adjust the HostName in the SSH configuration.
- Ensure the /var/www/html directory is owned securely in production environments.



