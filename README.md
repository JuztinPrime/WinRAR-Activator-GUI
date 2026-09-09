# WinRaR Activator GUI

> **This project is intended for educational and entertainment purposes only.**

A small Windows app (single `.exe`) that installs a WinRAR registration key
(`rarreg.key`) into your WinRAR folder, with a simple one-button interface.

*Script created by [NaeemBolchhi](https://github.com/NaeemBolchhi) · GUI created by JuztinPrime*

---

## What it is

WinRAR is trial ("nagware") software; installing a `rarreg.key` is how a
purchased licence is applied. This tool automates that step: it finds your
WinRAR folder, backs up any existing key, installs the new one, and restarts
WinRAR — all from one **ACTIVATE** button.

> ⚠️ **You are responsible for the key you install.** If you do not own a valid
> WinRAR licence, using someone else's key is software piracy and violates
> WinRAR's licence agreement. If you use WinRAR regularly, buy a licence at
> <https://www.win-rar.com>.

## Features

- One-button GUI with a live, CMD-style activity log
- Animated **ACTIVATE → ACTIVATING… → ACTIVATED!** button
- Automatic **light / dark theme** that follows Windows and switches live
- Automatic WinRAR-folder detection (Program Files, scoop, registry), with a
  manual folder picker as fallback
- Backs up the existing `rarreg.key` to `rarreg.key.bak` before replacing it
- Single self-contained executable — no installer, no runtime beyond .NET 4

## Requirements

| | |
|---|---|
| **OS** | Windows 7 SP1 or newer (built and tested on Windows 11) |
| **Runtime** | .NET Framework 4.x (already on Windows 8/10/11; may need installing on 7) |
| **Rights** | Administrator — it writes into `Program Files`, so it elevates via UAC |
| **Network** | Internet connection — this build downloads the key (see below) |

## Download
[WinRaR Activator GUI.zip](https://github.com/user-attachments/files/31996137/WinRaR.Activator.GUI.zip)

## Usage

### Automatic

> Automatic mode **requires an internet connection** — it downloads the key.
> With no connection it stops and reports *"no internet connection"* without
> changing anything; use the manual method instead.

1. Make sure WinRAR is installed.
2. Run `WinRaR Activator GUI.exe` and accept the UAC prompt.
3. The window shows the detected WinRAR folder. If WinRAR isn't found, click
   **ACTIVATE** and pick the folder that contains `WinRAR.exe`.
4. Click **ACTIVATE**. The log shows each step:
   - the WinRAR folder in use
   - the licence key downloaded from the configured URL
   - the existing `rarreg.key` backed up to `rarreg.key.bak`
   - the new key written into the WinRAR folder
   - WinRAR restarted so the change takes effect
5. The button turns green (**ACTIVATED!**) when it finishes.

### Manual (no internet)

1. Close WinRAR.
2. Copy `Manual Activation\rarreg.key` into your WinRAR program folder — usually
   `C:\Program Files\WinRAR` (Explorer asks for admin permission to copy there).
3. Start WinRAR.

## Configuration

The key's download URL can be set three ways, in order of precedence:

| Source | How |
|---|---|
| Command-line argument | `WinRaR Activator GUI.exe -url https://.../rarreg.key` |
| `keyurl.txt` beside the exe | first non-comment line is the `https` address |
| Built-in default | compiled in at build time via `build.ps1 -KeyUrl "https://..."` |

A runtime source (argument, then `keyurl.txt`) overrides the built-in URL.

## Building from source

No SDK, NuGet, or MSBuild required — just the in-box .NET Framework compiler:

```powershell
# plain build (no URL baked in)
powershell -ExecutionPolicy Bypass -File build.ps1

# bake a default key URL into the exe
powershell -ExecutionPolicy Bypass -File build.ps1 -KeyUrl "https://.../rarreg.key"
```

`build.ps1` compiles `src\Program.cs` with `csc.exe`, embedding `src\app.manifest`
(for the administrator elevation) and `src\app.ico`.

## Antivirus note

Microsoft Defender and other scanners may flag this exe (e.g.
`Trojan:Win32/Bearfoos.A!ml`). The `!ml` suffix means it's a **machine-learning
heuristic**, not a known-malware signature. It fires because the exe is small,
unsigned, asks for administrator rights, writes into `Program Files`, restarts
another process, and downloads a file — normal for what this tool does, but
together they resemble a downloader.

The exe is not infected, but because it downloads and installs a licence key,
some scanners will quarantine it regardless. If you trust it, restore it from
your antivirus's protection history and add an exclusion. **That is your
decision to make.** For distribution to other machines, code-signing the exe is
the only thing that reliably clears the heuristic.

## Troubleshooting

| Message / symptom | Fix |
|---|---|
| *Access denied … run as administrator* | Right-click the exe → **Run as administrator** |
| *WinRAR was not found* | Click **ACTIVATE** and select the folder containing `WinRAR.exe` |
| *no internet connection* / *could not reach the key URL* | Check your connection, or use manual activation |
| Download fails with a **404** | The URL/branch moved — use `keyurl.txt` or `-url`, or activate manually |
| A previous key was replaced | The old key is saved as `rarreg.key.bak` in the WinRAR folder; rename it back to restore |

## Disclaimer

This project is intended for **educational and entertainment purposes only**. It
is provided as-is, with no warranty. You are responsible for complying with
WinRAR's licence terms and the laws that apply to you. The authors are not
responsible for how you use it.
