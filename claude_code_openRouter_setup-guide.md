# Claude Code + OpenRouter Setup Guide
### Use Claude Code CLI with Free AI Models — No Anthropic Account Needed!

---

## 📋 Table of Contents
1. [What is This?](#what-is-this)
2. [Requirements](#requirements)
3. [Step 1: Install Claude Code](#step-1-install-claude-code)
4. [Step 2: Get Your OpenRouter API Key](#step-2-get-your-openrouter-api-key)
5. [Step 3: Connect Claude Code to OpenRouter](#step-3-connect-claude-code-to-openrouter)
6. [Step 4: Start Claude Code](#step-4-start-claude-code)
7. [Troubleshooting](#troubleshooting)
8. [Quick Reference](#quick-reference)

---

## What is This?

This guide helps you use **Claude Code** (Anthropic's powerful AI coding assistant) completely **free** by connecting it to **OpenRouter** — without needing an Anthropic account or credit card.

| Tool | Purpose |
|---|---|
| **Claude Code** | The AI coding assistant (interface) |
| **OpenRouter** | Provides free AI models (Qwen, Gemini, MiniMax, etc.) |

> **No Anthropic account needed. No credit card required.**

---

## Requirements

| Requirement | Details |
|---|---|
| Operating System | Windows with WSL (Ubuntu 22.04 or 24.04) |
| OpenRouter Account | Free account at openrouter.ai |

> **Don't have WSL?** Open Microsoft Store → Search **"Ubuntu"** → Install it for free.

---

## Step 1: Install Claude Code

Open your **WSL/Ubuntu terminal** and run:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

> **Windows PowerShell alternative:**
> ```powershell
> irm https://claude.ai/install.ps1 | iex
> ```

Verify installation:

```bash
claude --version
```

✅ You should see a version number like `2.1.126`.

---

## Step 2: Get Your OpenRouter API Key

1. Go to [https://openrouter.ai](https://openrouter.ai)
2. Click **Sign In** → **Continue with GitHub**
3. When asked about credits, click **"Maybe later! I'll start with free models."**
4. Click **Get API Key** in the top navigation
5. Click **New Key** button
6. Enter a name for your key (e.g. `claude-code`)
7. Click **Create**
8. Copy your key — it looks like: `sk-or-v1-xxxxxxxxxxxxxxxx`

> 💡 Keep your API key safe. Never share it with anyone.

---

## Step 3: Connect Claude Code to OpenRouter

### Navigate to Claude settings folder:

```bash
cd ~/.claude
ls
```

You will see `settings.json`. If it does not exist, create it:

```bash
touch settings.json
```

### Edit the settings file:

```bash
nano settings.json
```

Paste this configuration — **replace `<your-openrouter-api-key>` with your real API key:**

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://openrouter.ai/api",
    "ANTHROPIC_AUTH_TOKEN": "<your-openrouter-api-key>",
    "ANTHROPIC_API_KEY": ""
  }
}
```

Save the file:
- Press **Ctrl+O** → Enter (to save)
- Press **Ctrl+X** (to exit)

---

## Step 4: Start Claude Code

Open a **new terminal** and run:

```bash
claude
```

You will see a trust prompt — select **"Yes, I trust this folder"** and press Enter.

Claude Code will launch. You will see the model name in the bottom left corner.

**Test it** by typing:

```
hi
```

🎉 If you get a response, your setup is complete!

### Switching Models

To use a different free model, type `/model` inside Claude Code and select from the list.

**Popular free models on OpenRouter:**

| Model | Best For |
|---|---|
| `qwen/qwen3-235b-a22b:free` | Coding, general tasks |
| `qwen/qwen3-coder:free` | Coding specifically |
| `google/gemini-2.5-pro-exp-03-25:free` | Complex reasoning |
| `minimax/minimax-m2.5:free` | General tasks |

---
---

## Quick Reference

| Command | Description |
|---|---|
| `claude` | Launch Claude Code |
| `claude --version` | Show Claude Code version |
| `/model` | Switch AI model inside Claude Code |
| `/quit` | Exit Claude Code |
| `nano ~/.claude/settings.json` | Edit OpenRouter config |

---

## Versions Used in This Guide

| Tool | Version |
|---|---|
| Claude Code | v2.1.126 |
| OpenRouter | Latest |

---