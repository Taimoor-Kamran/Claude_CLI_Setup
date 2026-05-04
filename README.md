# Claude CLI Setup

> **Use Claude Code with Qwen Models — completely free!**
>
> This guide walks you through setting up Claude Code (Anthropic's AI coding assistant) to run on top of Qwen's free models. Even if you've never used a terminal before, follow each step carefully and you'll have it working in minutes.

---

## 📋 Table of Contents

1. [Requirements](#-requirements)
2. [Install Qwen CLI](#-qwen-cli-installation)
3. [Install Claude Code Router](#-step-1-install-claude-code-router)
4. [Get Your Qwen Access Token](#-step-2-get-your-qwen-access-token)
5. [Create Required Folders](#-step-3-create-required-folders)
6. [Create the Router Config](#-step-4-create-the-router-configuration)
7. [Start Using Claude Code](#-step-5-start-using-claude-code-with-qwen)
8. [Fixing 401 Token Errors](#-handling-401-token-errors)

---

## ⭐ Requirements

Before you start, make sure you have these two things installed on your computer:

| Requirement | Why you need it |
|---|---|
| **Qwen CLI** | Provides the free AI model that powers Claude Code |
| **Node.js v18 or later** | Required to run npm commands and install packages |

> **Don't have Node.js?** Download it from [nodejs.org](https://nodejs.org) and install the LTS version.

---

## 🧩 Qwen CLI Installation

**What is this?** Qwen CLI is the command-line tool for Alibaba's Qwen AI models. We'll use it as the free backend for Claude Code.

Open your terminal and run:

```bash
npm install -g @qwen-code/qwen-code@latest
```

> **What does `-g` mean?** It installs the package globally, so you can use the `qwen` command from anywhere on your computer.

After installation, confirm it worked:

```bash
qwen --version
```

You should see a version number printed. If you do, you're good to go!

---

## 🚀 Step 1: Install Claude Code Router

**What is this?** We need two packages:
- `@anthropic-ai/claude-code` — the Claude Code CLI tool itself
- `@musistudio/claude-code-router` (`ccr`) — a router that redirects Claude's requests to Qwen instead of Anthropic's servers

Run this single command to install both:

```bash
npm install -g @anthropic-ai/claude-code @musistudio/claude-code-router
```

---

## 🔑 Step 2: Get Your Qwen Access Token

**What is this?** When you logged into Qwen CLI, it saved a secret token on your computer. We need to copy that token into our router config so the router can talk to Qwen on your behalf.

### Find the token file

Open this file using Notepad (Windows):

```
C:\Users\PC_USER\.qwen\oauth_creds.json
```

> **Replace `PC_USER`** with your actual Windows username. For example: `C:\Users\John\.qwen\oauth_creds.json`

The file looks like this:

```json
{
  "access_token": "YOUR_QWEN_ACCESS_TOKEN_HERE",
  "token_type": "Bearer",
  "refresh_token": "YOUR_QWEN_REFRESH_TOKEN_HERE",
  "resource_url": "portal.qwen.ai",
  "expiry_date": 1764876220290
}
```

**Copy the value of `access_token`** (the long string between the quotes). You'll need it in the next step.

---

## 📁 Step 3: Create Required Folders

**What is this?** These are the folders where the router and Claude will store their config files.

Open your **WSL terminal** (Windows Subsystem for Linux) and run:

```bash
mkdir -p ~/.claude-code-router ~/.claude
```

> **What is WSL?** It's a Linux environment built into Windows. If you don't have it, search "WSL" in the Microsoft Store and install Ubuntu.
>
> The `~` symbol means your home folder (e.g. `/home/yourname`). The `-p` flag creates all folders in the path if they don't already exist — so it's safe to run even if the folders already exist.

---

## ⚙️ Step 4: Create the Router Configuration

**What is this?** This config file tells the router which AI provider to use (Qwen), where to reach it, and which model to use.

Paste the entire block below into your WSL terminal. **Before running it, replace `YOUR_QWEN_ACCESS_TOKEN_HERE` with the token you copied in Step 2.**

```bash
cat > ~/.claude-code-router/config.json << 'EOF'
{
  "LOG": true,
  "LOG_LEVEL": "info",
  "HOST": "127.0.0.1",
  "PORT": 3456,
  "API_TIMEOUT_MS": 600000,
  "Providers": [
    {
      "name": "qwen",
      "api_base_url": "https://portal.qwen.ai/v1/chat/completions",
      "api_key": "YOUR_QWEN_ACCESS_TOKEN_HERE",
      "models": [
        "qwen3-coder-plus",
        "qwen3-coder-plus",
        "qwen3-coder-plus"
      ]
    }
  ],
  "Router": {
    "default": "qwen,qwen3-coder-plus",
    "background": "qwen,qwen3-coder-plus",
    "think": "qwen,qwen3-coder-plus",
    "longContext": "qwen,qwen3-coder-plus",
    "longContextThreshold": 60000,
    "webSearch": "qwen,qwen3-coder-plus"
  }
}
EOF
```

> **Tip:** After running the command, you can verify the file was created correctly by running:
> ```bash
> cat ~/.claude-code-router/config.json
> ```

---

## ▶️ Step 5: Start Using Claude Code with Qwen

You're almost there! Now start the router and launch Claude Code.

**Start (or restart) the router:**

```bash
ccr restart
```

**Launch Claude Code with Qwen:**

```bash
ccr code
```

**Test that everything works by typing:**

```
> hi
```

If Qwen responds, your setup is complete! 🎉

---

## 🔄 Handling 401 Token Errors

A `401` error means your Qwen access token has expired. This happens periodically. Here's how to fix it in 3 steps:

### 1️⃣ Reauthenticate with Qwen

Delete the old credentials file and log in again:

```bash
qwen
```

This will open a browser login page. Sign in to regenerate a fresh token.

### 2️⃣ Update Your Router Config with the New Token

Open the router config in Notepad (run this in PowerShell):

```powershell
notepad "$env:USERPROFILE\.claude-code-router\config.json"
```

Find the `api_key` line and replace the old token with your new `access_token` from the updated `oauth_creds.json` file.

### 3️⃣ Restart the Router

```bash
ccr restart
```

> **Note for PowerShell users:** Make sure you're running commands in the correct shell. Some commands (like `ccr`) need to be run in WSL, while others (like the `notepad` command above) are for PowerShell/Windows.

---

## ❓ Common Questions

**Q: What is WSL and do I need it?**
Yes, WSL (Windows Subsystem for Linux) is required to run the `bash` commands in this guide. Install it from the Microsoft Store by searching "Ubuntu".

**Q: Is this really free?**
Yes! Qwen offers free API access via their CLI tool. This setup routes Claude Code's requests through Qwen's free tier.

**Q: What is `qwen3-coder-plus`?**
It's Qwen's latest coding-focused model — optimized for programming tasks, which makes it a great match for Claude Code.

---

> Don't forget to subscribe to the YouTube Channel **Subhan Kaladi** for more tutorials like this! 🎬
