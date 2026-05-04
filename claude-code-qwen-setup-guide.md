# Claude Code + Qwen Setup Guide
### Use Claude Code CLI with Qwen Paid Models — No Anthropic Account Needed!

---

## 📋 Table of Contents
1. [What is This?](#what-is-this)
2. [Requirements](#requirements)
3. [Step 1: Install Node.js](#step-1-install-nodejs)
4. [Step 2: Install Qwen CLI](#step-2-install-qwen-cli)
5. [Step 3: Install Claude Code + Router](#step-3-install-claude-code--router)
6. [Step 4: Get Your Qwen API Key](#step-4-get-your-qwen-api-key)
7. [Step 5: Create Config File](#step-5-create-config-file)
8. [Step 6: Start Claude Code](#step-6-start-claude-code)
9. [Troubleshooting](#troubleshooting)
10. [Quick Reference](#quick-reference)

---

## What is This?

This guide helps you use **Claude Code** (Anthropic's powerful AI coding assistant) using your **Qwen paid API key** — without needing an Anthropic account.

| Tool | Purpose |
|---|---|
| **Claude Code** | The AI coding assistant (interface) |
| **Claude Code Router (CCR)** | Connects Claude Code to Qwen models |
| **Qwen API** | Powers Claude Code with Qwen3 Coder Plus model |

> **No Anthropic account needed. Just your Qwen API key.**

---

## Requirements

| Requirement | Details |
|---|---|
| Operating System | Windows with WSL (Ubuntu 22.04 or 24.04) |
| Node.js | v18 or later |
| Qwen API Key | From Alibaba Cloud (paid plan) |

> **Don't have WSL?** Open Microsoft Store → Search **"Ubuntu"** → Install it for free.

---

## Step 1: Install Node.js

Open your **WSL/Ubuntu terminal** and run these commands one by one:

**Install NVM (Node Version Manager):**
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

**Reload terminal:**
```bash
source ~/.bashrc
```

**Install Node.js v20:**
```bash
nvm install 20
nvm use 20
```

**Verify installation:**
```bash
node --version
npm --version
```

✅ You should see `v20.x.x` for Node.js and `10.x.x` (or higher) for npm.

---

## Step 2: Install Qwen CLI

```bash
npm install -g @qwen-code/qwen-code@latest
```

**Verify installation:**
```bash
qwen --version
```

✅ You should see a version number like `0.15.6`.

---

## Step 3: Install Claude Code + Router

Install both tools globally with one command:

```bash
npm install -g @anthropic-ai/claude-code @musistudio/claude-code-router@latest
```

**Verify installation:**
```bash
claude --version
ccr --version
```

✅ Claude Code version number and CCR help menu should appear.

---

## Step 4: Get Your Qwen API Key

1. Go to [https://www.alibabacloud.com](https://www.alibabacloud.com) and log in
2. Navigate to **Model Studio** → **API Keys**
3. Click **Create API Key**
4. Copy the key — it looks like: `sk-xxxxxxxxxxxxxxxx`

> 💡 Keep your API key safe. Never share it with anyone.

---

## Step 5: Create Config File

**Create the required folders:**
```bash
mkdir -p ~/.claude-code-router ~/.claude
```

**Create the config file:**

Copy the command below, **replace `YOUR_QWEN_API_KEY_HERE` with your real API key**, then run it:

```bash
cat > ~/.claude-code-router/config.json << 'EOF'
{
  "LOG": true,
  "LOG_LEVEL": "info",
  "HOST": "127.0.0.1",
  "PORT": 3456,
  "Providers": [
    {
      "name": "qwen",
      "api_base_url": "https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions",
      "api_key": "YOUR_QWEN_API_KEY_HERE",
      "models": [
        "qwen3-coder-plus",
        "qwen-max",
        "qwen-plus"
      ]
    }
  ],
  "Router": {
    "default": "qwen,qwen3-coder-plus",
    "background": "qwen,qwen-plus",
    "think": "qwen,qwen-max",
    "longContext": "qwen,qwen3-coder-plus"
  }
}
EOF
```

**Verify the file was created correctly:**
```bash
cat ~/.claude-code-router/config.json
```

✅ You should see your config with your API key inside.

---

## Step 6: Start Claude Code

**Start the router:**
```bash
ccr restart
```

You should see:
```
✅ Service started successfully in the background.
```

**Launch Claude Code:**
```bash
ccr code
```

**Test it** by typing:
```
hi
```

🎉 If Qwen responds, your setup is complete!

---

## Troubleshooting

### ❌ `node` command not found
You need to install Node.js. Go back to [Step 1](#step-1-install-nodejs).

### ❌ `ccr` or `claude` command not found
Run the install command again:
```bash
npm install -g @anthropic-ai/claude-code @musistudio/claude-code-router@latest
```

### ❌ 401 Unauthorized Error
Your API key is wrong or expired. Get a new key from Alibaba Cloud and update the config:
```bash
nano ~/.claude-code-router/config.json
```
Replace the `api_key` value with your new key, save with **Ctrl+O**, exit with **Ctrl+X**, then restart:
```bash
ccr restart
```

### ❌ Connection Error / No Response
Make sure the router is running:
```bash
ccr status
```
If not running, start it:
```bash
ccr restart
```

### 🔄 How to Update CCR to Latest Version
```bash
npm install -g @musistudio/claude-code-router@latest
ccr restart
```

---

## Quick Reference

| Command | Description |
|---|---|
| `ccr restart` | Start or restart the router |
| `ccr code` | Launch Claude Code |
| `ccr status` | Check if router is running |
| `ccr --version` | Show CCR version |
| `/model` | Switch AI model inside Claude Code |
| `/quit` | Exit Claude Code |

---

## Versions Used in This Guide

| Tool | Version |
|---|---|
| Node.js | v20.20.2 |
| Qwen CLI | v0.15.6 |
| Claude Code | v2.1.126 |
| Claude Code Router | v2.0+ |

---

*Guide created for students. Last updated: May 2026.*
