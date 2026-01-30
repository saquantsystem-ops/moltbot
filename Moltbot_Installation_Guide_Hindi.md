# 🦞 Moltbot - संपूर्ण इंस्टॉलेशन और सेटअप गाइड (Complete Installation & Setup Guide in Hindi)

यह गाइड आपको **Moltbot** — एक Personal AI Assistant को अपने कंप्यूटर पर इंस्टॉल करने और चलाने की पूरी प्रक्रिया समझाती है। इसे **Windows** पर WSL2 के जरिए या सीधे **Source से Build** करके चलाया जा सकता है।

---

## 📋 विषय सूची (Table of Contents)

1. [Moltbot क्या है? (परिचय)](#1-moltbot-क्या-है-परिचय)
2. [सिस्टम आवश्यकताएं (System Requirements)](#2-सिस्टम-आवश्यकताएं-system-requirements)
3. [इंस्टॉलेशन के तरीके (Installation Methods)](#3-इंस्टॉलेशन-के-तरीके-installation-methods)
4. [Windows पर WSL2 सेटअप (Recommended for Windows)](#4-windows-पर-wsl2-सेटअप-recommended-for-windows)
5. [Node.js इंस्टॉल करना](#5-nodejs-इंस्टॉल-करना)
6. [pnpm पैकेज मैनेजर इंस्टॉल करना](#6-pnpm-पैकेज-मैनेजर-इंस्टॉल-करना)
7. [Source से Moltbot इंस्टॉल करना (From Source)](#7-source-से-moltbot-इंस्टॉल-करना-from-source)
8. [Quick Install (npm/pnpm Global)](#8-quick-install-npmpnpm-global)
9. [Onboarding Wizard चलाना](#9-onboarding-wizard-चलाना)
10. [Gateway को चलाना (Running the Gateway)](#10-gateway-को-चलाना-running-the-gateway)
11. [AI Model Configuration (API Keys)](#11-ai-model-configuration-api-keys)
12. [Channels Setup (WhatsApp, Telegram, Discord, आदि)](#12-channels-setup-whatsapp-telegram-discord-आदि)
13. [Docker से इंस्टॉलेशन (वैकल्पिक)](#13-docker-से-इंस्टॉलेशन-वैकल्पिक)
14. [Verification और Testing](#14-verification-और-testing)
15. [Troubleshooting (समस्या निवारण)](#15-troubleshooting-समस्या-निवारण)
16. [महत्वपूर्ण Commands की सूची](#16-महत्वपूर्ण-commands-की-सूची)
17. [अगले कदम (Next Steps)](#17-अगले-कदम-next-steps)

---

## 1. Moltbot क्या है? (परिचय)

**Moltbot** एक **Personal AI Assistant** है जो आप अपने खुद के devices पर चलाते हैं। यह आपको उन channels पर जवाब देता है जिनका आप पहले से उपयोग करते हैं:

- **Messaging Platforms:** WhatsApp, Telegram, Slack, Discord, Signal, iMessage, Microsoft Teams, Google Chat
- **Extension Channels:** BlueBubbles, Matrix, Zalo, WebChat
- **Voice Support:** macOS/iOS/Android पर बोलना और सुनना

### मुख्य विशेषताएं:
- 🏠 **Local-first:** आपके डेटा आपके पास रहता है
- 🤖 **Multi-AI Support:** Claude, GPT, Bedrock, Ollama आदि
- 📱 **Multi-platform:** macOS, Linux, Windows (WSL2)
- 🔌 **Extensible:** Skills और Plugins से functionality बढ़ाएं

---

## 2. सिस्टम आवश्यकताएं (System Requirements)

### ✅ आवश्यक (Required):
| Component | Requirement |
|-----------|-------------|
| **Node.js** | Version **22 या उससे ऊपर** |
| **Operating System** | macOS, Linux, या Windows (WSL2 के साथ) |
| **RAM** | कम से कम 4GB (8GB+ recommended) |
| **Disk Space** | कम से कम 2GB free space |

### ⚡ Recommended (सर्वश्रेष्ठ अनुभव के लिए):
| Component | Recommendation |
|-----------|----------------|
| **Package Manager** | pnpm (source से build के लिए) |
| **AI Model** | Anthropic API Key या OpenAI API Key |
| **Web Search** | Brave Search API Key (optional) |

### ⚠️ Windows Users के लिए Important Note:
Native Windows पर Moltbot अभी **untested** है और कई समस्याएं हो सकती हैं। **WSL2 (Windows Subsystem for Linux 2)** का उपयोग करना **strongly recommended** है।

---

## 3. इंस्टॉलेशन के तरीके (Installation Methods)

Moltbot को इंस्टॉल करने के 4 मुख्य तरीके हैं:

| तरीका | कठिनाई | कब उपयोग करें |
|-------|--------|----------------|
| **Quick Install (npm)** | 🟢 आसान | जल्दी शुरू करने के लिए |
| **Source से Build** | 🟡 मध्यम | Developers और customization के लिए |
| **Docker** | 🟡 मध्यम | Container-based deployment के लिए |
| **Nix** | 🔴 Advanced | Declarative configuration के लिए |

---

## 4. Windows पर WSL2 सेटअप (Recommended for Windows)

Windows users को पहले **WSL2** सेटअप करना होगा।

### Step 4.1: WSL2 Install करें

**PowerShell (Administrator)** खोलें और ये commands चलाएं:

```powershell
# WSL2 और Ubuntu इंस्टॉल करें
wsl --install

# या specific distro चुनें
wsl --list --online
wsl --install -d Ubuntu-24.04
```

> ⚠️ **Note:** इंस्टॉलेशन के बाद Windows **Restart** करना पड़ सकता है।

### Step 4.2: Ubuntu में Username और Password बनाएं

WSL first time खोलने पर आपसे username और password मांगा जाएगा।

### Step 4.3: Systemd Enable करें (Gateway Service के लिए जरूरी)

Ubuntu Terminal में ये commands चलाएं:

```bash
# wsl.conf फाइल बनाएं
sudo tee /etc/wsl.conf > /dev/null <<'EOF'
[boot]
systemd=true
EOF
```

फिर PowerShell में:

```powershell
# WSL को restart करें
wsl --shutdown
```

Ubuntu फिर से खोलें और verify करें:

```bash
systemctl --user status
```

अगर कोई error नहीं आए तो systemd सही से काम कर रहा है।

---

## 5. Node.js इंस्टॉल करना

### 🪟 Windows (WSL2 में) / 🐧 Linux / 🍎 macOS

**Recommended Method: nvm (Node Version Manager)**

```bash
# nvm इंस्टॉल करें
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash

# Terminal restart करें या source करें
source ~/.bashrc  # या ~/.zshrc अगर zsh use करते हैं

# Node.js 22 इंस्टॉल करें
nvm install 22

# Node.js 22 को default बनाएं
nvm alias default 22

# Verify करें
node -v  # Output: v22.x.x होना चाहिए
npm -v   # npm भी साथ आएगा
```

### Alternative: Official Package Manager से

**Ubuntu/Debian:**
```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs
```

**macOS (Homebrew):**
```bash
brew install node@22
```

---

## 6. pnpm पैकेज मैनेजर इंस्टॉल करना

Source से build करने के लिए `pnpm` चाहिए:

```bash
# corepack enable करें (Node.js 22+ में built-in)
corepack enable

# या npm से install करें
npm install -g pnpm

# Verify करें
pnpm -v
```

---

## 7. Source से Moltbot इंस्टॉल करना (From Source)

यह method **developers** और **customization** के लिए है।

### Step 7.1: Repository Clone करें

```bash
# Home directory में जाएं (optional)
cd ~

# Moltbot clone करें
git clone https://github.com/moltbot/moltbot.git

# Directory में जाएं
cd moltbot
```

### Step 7.2: Dependencies Install करें

```bash
# सभी dependencies install करें
pnpm install
```

> ⏱️ **Note:** पहली बार install करने में 2-5 मिनट लग सकते हैं।

### Step 7.3: UI Build करें

```bash
# UI dependencies install और build करें
pnpm ui:build
```

### Step 7.4: Main Application Build करें

```bash
# TypeScript को JavaScript में compile करें
pnpm build
```

### 🎉 Source से Installation पूर्ण!

Build successful होने पर आपको `dist/` folder में compiled code मिलेगा।

---

## 8. Quick Install (npm/pnpm Global)

अगर आप source से build नहीं करना चाहते, तो globally install करें:

### 🖥️ Linux/macOS/WSL2:

**One-liner Installer (Recommended):**
```bash
curl -fsSL https://molt.bot/install.sh | bash
```

**या npm/pnpm से:**
```bash
# npm से
npm install -g moltbot@latest

# या pnpm से
pnpm add -g moltbot@latest
```

### 🪟 Windows (PowerShell):

```powershell
iwr -useb https://molt.bot/install.ps1 | iex
```

### Verify Installation:

```bash
moltbot --version
```

---

## 9. Onboarding Wizard चलाना

Onboarding wizard आपको step-by-step setup करने में मदद करता है।

### Step 9.1: Onboarding शुरू करें

**Global Install से:**
```bash
moltbot onboard --install-daemon
```

**Source से (अगर global install नहीं किया):**
```bash
pnpm moltbot onboard --install-daemon
```

### Step 9.2: Wizard में ये चुनाव करें:

1. **Gateway Mode:** Local vs Remote (Local recommended for beginners)

2. **Authentication Provider:**
   - **Anthropic (Claude)** - API Key डालें
   - **OpenAI (GPT)** - API Key डालें
   - **Synthetic** - Testing के लिए (real AI नहीं)

3. **Channels:** (Optional - बाद में भी setup कर सकते हैं)
   - WhatsApp - QR code scan करेंगे
   - Telegram - Bot token डालेंगे
   - Discord - Bot token डालेंगे

4. **Daemon Installation:** Yes (recommended - background में चलेगा)

### Step 9.3: Configuration File Location

Wizard एक config file बनाएगा:
- **Path:** `~/.clawdbot/moltbot.json`

---

## 10. Gateway को चलाना (Running the Gateway)

Gateway Moltbot का "control plane" है जो सभी connections manage करता है।

### Daemon के साथ (Background Service):

अगर onboarding में `--install-daemon` चुना था:

```bash
# Status check करें
moltbot gateway status

# Start करें (अगर running नहीं है)
moltbot gateway start

# Stop करें
moltbot gateway stop
```

### Manual Foreground Mode:

```bash
# Verbose mode में चलाएं (debugging के लिए)
moltbot gateway --port 18789 --verbose
```

**Source से:**
```bash
# Development mode (auto-reload on changes)
pnpm gateway:watch

# या simply
node moltbot.mjs gateway --port 18789 --verbose
```

### Dashboard Access:

Browser में खोलें: `http://127.0.0.1:18789/`

---

## 11. AI Model Configuration (API Keys)

Moltbot को काम करने के लिए AI model की API Key चाहिए।

### 🔵 Anthropic (Claude) - Recommended

1. [Anthropic Console](https://console.anthropic.com/) पर जाएं
2. Sign up / Login करें
3. API Keys section में जाएं
4. "Create Key" click करें
5. Key copy करें

**Configuration में add करें:**

```bash
# Interactive way
moltbot configure --section auth

# या manually moltbot.json में
```

`~/.clawdbot/moltbot.json`:
```json
{
  "agent": {
    "model": "anthropic/claude-opus-4-5"
  }
}
```

### 🟢 OpenAI (GPT)

1. [OpenAI Platform](https://platform.openai.com/) पर जाएं
2. API Keys section में जाएं
3. "Create new secret key" click करें
4. Key copy करें

### ⚙️ Environment Variable से:

```bash
# Anthropic
export ANTHROPIC_API_KEY="sk-ant-..."

# OpenAI
export OPENAI_API_KEY="sk-..."
```

---

## 12. Channels Setup (WhatsApp, Telegram, Discord, आदि)

### 📱 WhatsApp Setup

WhatsApp Web की तरह QR code से link करें:

```bash
moltbot channels login
```

1. Terminal में QR code दिखेगा
2. WhatsApp App खोलें
3. Settings > Linked Devices > Link a Device
4. QR code scan करें

**Configuration:**
`~/.clawdbot/moltbot.json`:
```json
{
  "channels": {
    "whatsapp": {
      "enabled": true,
      "allowFrom": ["+91XXXXXXXXXX"]
    }
  }
}
```

### ✈️ Telegram Setup

1. [@BotFather](https://t.me/botfather) से बात करें
2. `/newbot` command भेजें
3. Bot का नाम और username दें
4. Bot Token मिलेगा (जैसे: `123456789:ABCdefGHIjklmNOPqrs`)

**Configuration:**

```bash
# Environment variable
export TELEGRAM_BOT_TOKEN="123456789:ABCdefGHIjklmNOPqrs"
```

या `moltbot.json` में:
```json
{
  "channels": {
    "telegram": {
      "botToken": "123456789:ABCdefGHIjklmNOPqrs"
    }
  }
}
```

### 🎮 Discord Setup

1. [Discord Developer Portal](https://discord.com/developers/applications) पर जाएं
2. "New Application" बनाएं
3. Bot section में जाएं
4. "Add Bot" click करें
5. Token copy करें

```json
{
  "channels": {
    "discord": {
      "token": "your-discord-bot-token"
    }
  }
}
```

### 💬 Slack Setup

1. [Slack API](https://api.slack.com/apps) पर जाएं
2. "Create New App" > "From scratch"
3. Bot Token Scopes add करें:
   - `chat:write`
   - `channels:read`
   - `channels:history`
4. Install to workspace
5. Bot Token और App Token copy करें

```bash
export SLACK_BOT_TOKEN="xoxb-..."
export SLACK_APP_TOKEN="xapp-..."
```

---

## 13. Docker से इंस्टॉलेशन (वैकल्पिक)

Docker से चलाने के लिए:

### Step 13.1: Docker Image Build करें

```bash
cd moltbot  # repository में जाएं
docker build -t moltbot:local .
```

### Step 13.2: Environment Variables तैयार करें

`.env` file बनाएं:

```bash
CLAWDBOT_CONFIG_DIR=/path/to/.clawdbot
CLAWDBOT_WORKSPACE_DIR=/path/to/clawd
CLAWDBOT_GATEWAY_PORT=18789
```

### Step 13.3: Docker Compose से चलाएं

```bash
docker-compose up -d moltbot-gateway
```

### Docker Compose Configuration:

```yaml
services:
  moltbot-gateway:
    image: moltbot:local
    environment:
      HOME: /home/node
      TERM: xterm-256color
    volumes:
      - ${CLAWDBOT_CONFIG_DIR}:/home/node/.clawdbot
      - ${CLAWDBOT_WORKSPACE_DIR}:/home/node/clawd
    ports:
      - "18789:18789"
    init: true
    restart: unless-stopped
    command: ["node", "dist/index.js", "gateway", "--bind", "lan", "--port", "18789"]
```

---

## 14. Verification और Testing

### 🩺 Health Check Commands:

```bash
# Overall status
moltbot status

# Detailed status (pasteable for debugging)
moltbot status --all

# Health of running gateway
moltbot health

# Security audit
moltbot security audit --deep
```

### ✅ Expected Outputs:

**moltbot status:**
```
✓ Gateway: Running on port 18789
✓ Auth: Configured (Anthropic)
✓ Channels: WhatsApp (connected)
```

### 📨 Test Message भेजें:

```bash
moltbot message send --target "+91XXXXXXXXXX" --message "Hello from Moltbot!"
```

### 🔧 Doctor Command (Issues diagnose करने के लिए):

```bash
moltbot doctor
```

---

## 15. Troubleshooting (समस्या निवारण)

### ❌ Problem: `moltbot: command not found`

**Solution:**
```bash
# npm global path check करें
npm prefix -g

# PATH में add करें
export PATH="$(npm prefix -g)/bin:$PATH"

# .bashrc या .zshrc में add करें (permanent fix)
echo 'export PATH="$(npm prefix -g)/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### ❌ Problem: `sharp: error / node-gyp error`

**Solution:**
```bash
# Global libvips को ignore करें
SHARP_IGNORE_GLOBAL_LIBVIPS=1 npm install -g moltbot@latest
```

### ❌ Problem: WhatsApp QR code scan के बाद भी connect नहीं हो रहा

**Solution:**
1. पुराने sessions clear करें:
   ```bash
   rm -rf ~/.clawdbot/credentials/whatsapp*
   ```
2. फिर से `moltbot channels login` चलाएं

### ❌ Problem: "no auth configured" error

**Solution:**
API Key configure करें (Step 11 देखें)

### ❌ Problem: DM में bot reply नहीं कर रहा

**Solution:** Pairing approve करें:
```bash
# Pending pairings देखें
moltbot pairing list whatsapp

# Approve करें
moltbot pairing approve whatsapp <code>
```

### ❌ Problem: Windows पर bash scripts fail हो रहे

**Solution:** 
- WSL2 use करें (recommended)
- या Git Bash में try करें

---

## 16. महत्वपूर्ण Commands की सूची

### 🚀 Starting/Stopping:

| Command | Description |
|---------|-------------|
| `moltbot onboard` | Setup wizard |
| `moltbot gateway` | Gateway start (foreground) |
| `moltbot gateway start` | Gateway start (daemon) |
| `moltbot gateway stop` | Gateway stop |
| `moltbot gateway status` | Gateway status |

### 📊 Status & Health:

| Command | Description |
|---------|-------------|
| `moltbot status` | Quick status |
| `moltbot status --all` | Detailed status |
| `moltbot health` | Health check |
| `moltbot doctor` | Diagnose issues |

### 📱 Channels:

| Command | Description |
|---------|-------------|
| `moltbot channels login` | WhatsApp QR login |
| `moltbot channels status` | Channels status |
| `moltbot pairing list <channel>` | Pending pairings |
| `moltbot pairing approve <channel> <code>` | Approve pairing |

### 💬 Messaging:

| Command | Description |
|---------|-------------|
| `moltbot message send --target <number> --message "text"` | Send message |
| `moltbot agent --message "query"` | Direct agent query |

### ⚙️ Configuration:

| Command | Description |
|---------|-------------|
| `moltbot configure` | Interactive config |
| `moltbot config set key value` | Set config value |
| `moltbot dashboard` | Open web dashboard |

### 🔧 Development (Source build):

| Command | Description |
|---------|-------------|
| `pnpm install` | Install dependencies |
| `pnpm build` | Build TypeScript |
| `pnpm ui:build` | Build UI |
| `pnpm gateway:watch` | Dev mode with auto-reload |
| `pnpm test` | Run tests |
| `pnpm lint` | Lint code |

---

## 17. अगले कदम (Next Steps)

### 🎯 Setup Complete होने के बाद:

1. **📱 macOS App (Optional):**
   - Menu bar control
   - Voice Wake feature
   - Canvas support
   - [Docs](https://docs.molt.bot/platforms/macos)

2. **📲 Mobile Nodes (iOS/Android):**
   - Camera control
   - Location access
   - Notifications
   - [Docs](https://docs.molt.bot/nodes)

3. **🌐 Remote Access:**
   - Tailscale Serve/Funnel
   - SSH Tunnels
   - [Docs](https://docs.molt.bot/gateway/remote)

4. **🛠️ Skills & Automation:**
   - Custom skills बनाएं
   - Cron jobs setup करें
   - Webhooks integrate करें
   - [Docs](https://docs.molt.bot/tools/skills)

5. **🔒 Security Hardening:**
   - DM policies configure करें
   - Sandbox mode enable करें
   - [Docs](https://docs.molt.bot/gateway/security)

---

## 📚 Additional Resources

- **Official Documentation:** https://docs.molt.bot
- **GitHub Repository:** https://github.com/moltbot/moltbot
- **Discord Community:** https://discord.gg/clawd
- **Getting Started Guide:** https://docs.molt.bot/start/getting-started
- **Configuration Reference:** https://docs.molt.bot/gateway/configuration

---

## ✅ Quick Start Summary (त्वरित सारांश)

```bash
# 1. Node.js 22+ install करें
nvm install 22

# 2. pnpm install करें
npm install -g pnpm

# 3. Moltbot clone और build करें
git clone https://github.com/moltbot/moltbot.git
cd moltbot
pnpm install
pnpm ui:build
pnpm build

# 4. Onboarding wizard चलाएं
pnpm moltbot onboard --install-daemon

# 5. Gateway start करें
pnpm gateway:watch

# 6. Dashboard खोलें
# Browser में: http://127.0.0.1:18789/
```

---

**🎉 बधाई हो! आपने Moltbot successfully install और configure कर लिया है!**

अगर कोई समस्या आए तो [Troubleshooting](#15-troubleshooting-समस्या-निवारण) section देखें या Discord community से मदद लें।

---
*यह गाइड Moltbot version 2026.1.27-beta.1 के लिए बनाई गई है।*
*Last Updated: 30 January, 2026*
