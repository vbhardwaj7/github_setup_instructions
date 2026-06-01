# GitHub SSH Setup Guide (Windows)

This guide provides the exact steps to configure GitHub access using SSH on a Windows machine. You can provide this file to an AI assistant (like Antigravity) to automate the setup.

## Prerequisites
- Git installed on the local machine.
- A GitHub account.

## Setup Steps

### 1. Configure Global Git Identity
Set your name and email if not already configured.
```powershell
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

### 2. Generate a New SSH Key
Generate a modern Ed25519 key.
```powershell
# Create .ssh directory if it doesn't exist
mkdir ~/.ssh -Force

# Generate the key (No passphrase for convenience)
ssh-keygen -t ed25519 -C "your-email@example.com" -f "$env:USERPROFILE\.ssh\id_ed25519" -N '""'
```

### 3. Start SSH Agent and Add Key
This requires **Administrator** privileges to run the service commands.
```powershell
# Set service to start automatically and start it
Set-Service ssh-agent -StartupType Automatic
Start-Service ssh-agent

# Add your private key to the agent
ssh-add "$env:USERPROFILE\.ssh\id_ed25519"
```

### 4. Add Public Key to GitHub
1. Copy the output of this command:
   ```powershell
   cat "$env:USERPROFILE\.ssh\id_ed25519.pub"
   ```
2. Go to [GitHub SSH Settings](https://github.com/settings/keys).
3. Click **New SSH Key**, give it a title, and paste the content.

### 5. Verify Connection
Run this to confirm setup:
```powershell
ssh -T git@github.com -o StrictHostKeyChecking=accept-new
```
You should see: *"Hi [username]! You've successfully authenticated..."*
