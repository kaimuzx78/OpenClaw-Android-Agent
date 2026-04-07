# Open-Claw Android Agent

> **Run a real AI agent directly on your Android phone — no root, no PC, no limits.**

Turn your phone into a fully autonomous AI-controlled device using Termux, Shizuku, and the Gemini API. The agent can read your screen, tap buttons, open apps, scroll, type, and execute any shell command — all from a natural language prompt.

---

## How It Works

```
You type a message
       ↓
  Gemini AI thinks
       ↓
  phone_control.sh runs
       ↓
 Shizuku executes on device
       ↓
  Your phone responds
```

No cloud connector. No remote server. The entire pipeline runs **on the phone itself** via Termux.

---

## What You Need Before Starting

- Android **11 or higher**
- **Termux** — Install from [F-Droid](https://f-droid.org/en/packages/com.termux/) only. The Play Store version is outdated and will not work.
- **Shizuku** — Available on [F-Droid](https://f-droid.org/packages/moe.shizuku.privileged.api/) or [Play Store](https://play.google.com/store/apps/details?id=moe.shizuku.privileged.api)
- **Gemini API Key** — Completely free at [aistudio.google.com](https://aistudio.google.com/apikey)

---

## Installation

### 1. Enable Developer Options

Go to `Settings → About Phone` and tap **Build Number** 7 times.
Then go to `Developer Options` and enable **Wireless Debugging**.

### 2. Set Up Shizuku

1. Open the **Shizuku** app
2. Tap **Pairing** → then in your phone's Wireless Debugging settings, tap **Pair device with pairing code**
3. Enter the code shown — Shizuku will say **"Shizuku is running"**
4. Tap **"Use Shizuku in terminal apps"** → **"Export files"**
5. When asked where to save, navigate to your **internal storage root** and create a folder called `Shizuku`

> The folder name is case-sensitive. It must be `Shizuku` with a capital S.

### 3. Run the Installer

Open **Termux** and run:

```bash
curl -sL https://raw.githubusercontent.com/kaimuzx78/OpenClaw-Android-Agent/main/auto_setup.sh | bash
```

The script will handle everything else automatically — packages, Shizuku linking, Node.js config, and AI brain setup.

### 4. Connect Your AI

```bash
openclaw onboard
openclaw auth add google --key YOUR_GEMINI_KEY
```

### 5. Start the Agent

```bash
openclaw gateway
```

The AI is now active and listening. Message it and watch it work.

---

## What the Agent Can Do

| Action | How to ask |
|---|---|
| Check battery | *"What's my battery level?"* |
| Open an app | *"Open Instagram"* |
| Navigate the UI | *"Go to Display settings and enable dark mode"* |
| Take a screenshot | *"Screenshot the current screen"* |
| Search YouTube | *"Search YouTube for synthwave music"* |
| Toggle WiFi | *"Turn off WiFi"* |
| Type text | *"Type hello world in the search bar"* |
| Run any shell command | *"Run df -h and tell me my storage usage"* |
| Scroll the screen | *"Scroll down to find the logout button"* |
| Go home / back | *"Go to the home screen"* |

The agent automatically reads your screen using `ui-dump`, locates the target element, calculates its coordinates, and taps or interacts with it.

---

## Troubleshooting

**`rish: command not found`**
The Shizuku scripts weren't linked. Re-run:
```bash
bash ~/storage/shared/Shizuku/copy.sh
```

**`rish_shizuku.dex not found`**
You need to re-export the files from Shizuku. Open the app → Use Shizuku in terminal apps → Export files → save to the `Shizuku` folder in internal storage.

**Shizuku stops working after reboot**
Shizuku needs to be restarted after every reboot. Open the Shizuku app and tap Start, or run `shizuku` in Termux. You can automate this with Termux:Boot.

**Node.js can't connect / DNS errors**
```bash
export NODE_OPTIONS=--dns-result-order=ipv4first
```
Add to `~/.bashrc` to make it permanent.

**Nothing responds at all**
Run `rish -c whoami` in Termux. You should see `shell`. If not, Shizuku needs to be re-initialized.

---

## Project Structure

```
auto_setup.sh          ← Installer (run this first)
phone_control.sh       ← Generated at install time, handles all device commands
~/.openclaw/
  workspace/
    IDENTITY.md        ← Who the AI thinks it is
    TOOLS.md           ← What tools the AI knows it has
    AGENTS.md          ← Behavioral rules for the agent loop
```

---

## Built On

| Tool | Role |
|---|---|
| [OpenClaw](https://github.com/AidanPark/openclaw-android) | AI agent runtime & LLM gateway |
| [Shizuku](https://github.com/RikkaApps/Shizuku) | ADB-level system access without root |
| [Termux](https://github.com/termux/termux-app) | Linux environment on Android |
| [Google Gemini](https://ai.google.dev/) | Language model powering the agent |

---

## License

MIT — do whatever you want with it.