# Termux Live Screen Reader

**Natively see and read your Android phone's live screen/UI from Termux** — no root, no USB cable, no wireless ADB, no PC required.

This repository provides the practical setup guide and references to make your Termux terminal able to capture screenshots and dump the live UI hierarchy of whatever is currently displayed on your phone.

## Why this works natively on-device

Android's AccessibilityService API (available since early versions, with silent screenshots from API 33 / Android 13+) allows a small helper app to:
- Capture the full screen silently (`takeScreenshot()`)
- Dump the complete UI node tree (text, bounds, resource-ids, clickable elements)
- Perform taps, scrolls, typing, etc.

The recommended implementation is **AccessDroid** (or the closely related CostPhone fork). Once the AccessibilityService is enabled, your Termux scripts and agents can call `amctl` to see and interact with the live display.

## Requirements

- Android 7.0+ (API 24). Screenshots via AccessibilityService require **Android 13+ (API 33)**.
- Termux (F-Droid version recommended)
- `termux-api` package + Termux:API app
- One-time enable of the AccessibilityService in Settings

## Quick Start (Recommended)

### 1. Install AccessDroid (one-liner)

```bash
bash <(curl -sL https://raw.githubusercontent.com/yashas-13/accessdroid-termux/master/accessdroid/install.sh)
```

This builds the APK, installs it, and guides you through enabling the service.

### 2. Enable the AccessibilityService

Settings → Accessibility → Downloaded services (or Installed apps) → AccessDroid → Turn ON.

Re-enable after every reboot.

### 3. Enable Termux AM socket

Termux → Settings → Termux:API → "Allow am command socket" → ON. Restart Termux.

### 4. Test live screen access

```bash
amctl screenshot          # Capture current screen (saved as image)
amctl tree                # Dump UI hierarchy (check logcat for output)
amctl snap                # Text snapshot of interactive elements (CostPhone style)
```

For continuous monitoring:

```bash
for i in $(seq 1 5); do amctl screenshot; sleep 2; done
```

## Alternative: CostPhone

A Spanish-language fork with excellent OpenCode skill integration and `amctl snap` / `amctl see` commands:

```bash
git clone https://github.com/CostSquare99109/costphone
cd costphone && bash install.sh
```

Then enable CostPhone AccessibilityService the same way.

## What you get

| Command              | What it does                              |
|----------------------|-------------------------------------------|
| `amctl screenshot`   | Silent full-screen capture (API 33+)     |
| `amctl tree`         | Full UI node tree                         |
| `amctl snap`         | Readable text snapshot with element refs  |
| `amctl click "Login"`| Tap element by visible text               |
| `amctl type "hello"` | Type into focused field                   |
| `amctl scrollto "X"` | Scroll until text is visible              |

## Use cases for AI agents / gigs

- Vision-language model (VLM) loops that see the screen and decide next actions
- Automated testing / UI verification
- Building custom Grok skills that operate your phone
- Screen monitoring without a second device

## Limitations & Notes

- AccessibilityService must be manually re-enabled after reboot.
- Some banking/secure apps block screenshots or Accessibility.
- Battery optimization should be disabled for Termux and the helper app for reliability.
- This does **not** require root. It uses official Android APIs.

## Credits

- Primary: [yashas-13/accessdroid-termux](https://github.com/yashas-13/accessdroid-termux)
- Alternative: [CostSquare99109/costphone](https://github.com/CostSquare99109/costphone)

Clone this repo on your phone in Termux, follow the Quick Start, and your terminal will natively see the live display.

---

*Created for Captain Jack Sparrow — superior employer forward-step execution.*
