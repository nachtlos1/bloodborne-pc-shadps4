# Running Bloodborne on PC with the shadPS4 Emulator

Bloodborne is a PlayStation 4 exclusive with no official PC release. This project documents how I ran **my own copy** of the game on my Windows PC using **shadPS4**, an open-source PS4 emulator, along with the setup, settings and troubleshooting involved.

> **Legal note:** this repository contains **no game files, firmware, system modules or keys**, and doesn't link to any. Everything was dumped from a PS4 and a game I own. The emulator itself is open-source software. If you follow this guide, use only your own legally owned copies.

---

## Contents

1. [My PC](#my-pc)
2. [How emulation works](#how-emulation-works)
3. [What you need](#what-you-need)
4. [Setup](#setup)
5. [Settings](#settings)
6. [Troubleshooting](#troubleshooting)
7. [My results](#my-results)
8. [What I learned](#what-i-learned)

---

## My PC

| Component | Part |
|---|---|
| CPU | AMD Ryzen 7 7800X3D |
| GPU | MSI GeForce RTX 5070 Ventus 3X 12 GB |
| RAM | 32 GB DDR5 6000 MHz |
| Storage | 1 TB NVMe SSD |
| OS | Windows 11 |

(Full build details are in my [PC Build & Troubleshooting Guide](https://github.com/nachtlos1/pc-build-troubleshooting-guide).)

---

## How emulation works

A PS4 uses an x86-64 AMD CPU, like a PC, but a different operating system and GPU. shadPS4 does not run a copy of the PS4's operating system. Instead it:

- **Loads the game's code** and runs it directly on the PC's x86-64 CPU.
- **Re-implements PS4 system libraries** so that when the game asks the "PS4" to do something (load a file, play audio, read the controller), shadPS4 does it with Windows instead.
- **Translates PS4 graphics commands into Vulkan**, which the PC's GPU understands. Shaders are converted the first time they're needed, which is why the first run of a new area can stutter.

Some PS4 system libraries can be loaded from **modules dumped from a real PS4**, which can improve compatibility.

---

## What you need

| Item | Where it comes from |
|---|---|
| shadPS4 | Official GitHub releases: [shadps4-emu/shadPS4](https://github.com/shadps4-emu/shadPS4) |
| Bloodborne game dump | Dumped from **my own** disc/console |
| Game update (patch 1.09) | Dumped from **my own** console. Patch 1.09 is the version the community recommends for shadPS4 |
| System modules (optional) | Dumped from **my own** PS4's `/system/common/lib/` folder |
| Vulkan-capable GPU and up-to-date drivers | NVIDIA driver update |

**Title IDs:** Bloodborne's folder name depends on region. My copy is the European version, `CUSA00900` (the US version is `CUSA00207`).

---

## Setup

1. **Update the GPU driver.** shadPS4 renders through Vulkan, so an up-to-date NVIDIA driver avoids many crashes.
2. **Download shadPS4** from the official GitHub releases page and extract it to a folder (e.g. `C:\Emulators\shadPS4`). If it won't open on Windows, install or repair the Microsoft Visual C++ Redistributable.
3. **Set the game install folder.** On first launch, shadPS4 asks for a games directory. A simple option is a `games` folder inside the shadPS4 folder.
4. **Add the game dump.** Copy the dumped game folder into the games directory with its Title ID as the folder name:
   ```
   games/
   ├── CUSA00900/                  ← base game
   └── CUSA00900-UPDATE/           ← patch 1.09
   ```
   (The update folder name format is `CUSAxxxxx-UPDATE` or `CUSAxxxxx-patch`, matching the base game's ID.)
5. **Add system modules (optional).** Copy the dumped modules into shadPS4's `sys_modules` folder. To find it, right-click the game in shadPS4, choose *Open Folder → Open Log Folder*, then go up one level.
6. **Refresh the game list.** Bloodborne should appear. Check that the version shows **1.09**.
7. **Launch** by double-clicking the game.

---

## Settings

| Setting | Recommended starting point | Why |
|---|---|---|
| Graphics backend | Vulkan | shadPS4's renderer |
| Fullscreen | Borderless fullscreen | Smooth alt-tab, no mode switching |
| Present mode / V-Sync | V-Sync on | Prevents screen tearing |
| Resolution | 1080p (game default) | Most stable starting point |
| Controller | Wired controller | Map buttons in shadPS4's controller settings |

Community patches (for example 60 FPS or resolution patches) can be enabled through shadPS4's patch manager. Change one thing at a time and test after each change.

---

## Troubleshooting

I used the same method as my [PC troubleshooting guide](https://github.com/nachtlos1/pc-build-troubleshooting-guide): change one thing at a time and check the log.

| Problem | Likely cause | Fix |
|---|---|---|
| shadPS4 won't open | Missing Visual C++ runtime | Install/repair the VC++ Redistributable |
| Game doesn't appear in the list | Wrong folder structure or name | Check the folder is named after the Title ID and sits directly in the games directory |
| Crash on launch | Outdated GPU driver or wrong game version | Update the driver; confirm the update is 1.09 |
| Low or unstable FPS on strong hardware | GPU not getting full power (cables, power settings) | Check the GPU's power connectors and Windows/NVIDIA power settings. This was the fix on my PC |
| Stutter in new areas | Shaders compiling for the first time | Normal on first visit; improves after |
| Graphical glitches | Emulator bug or setting | Update shadPS4, try default settings, check the [compatibility list](https://github.com/shadps4-compatibility/shadps4-game-compatibility) |
| Any other issue | Unknown | Read the shadPS4 log file (*Open Folder → Open Log Folder*) for the first error |

---

## My results

| | |
|---|---|
| Game | Bloodborne, European release (`CUSA00900`), dumped from my own PS4 and disc |
| PC | Ryzen 7 7800X3D, RTX 5070 12 GB, 32 GB DDR5, Windows 11 |
| Frame rate | **30–60 FPS** |

**Problem I solved:** performance was held back by a **GPU power issue**. I traced it back to the graphics card's power and fixed it, which brought performance to where it should be on an RTX 5070. It was a good reminder to check the hardware basics before blaming the software.

---

## What I learned

- **How emulation works:** CPU code can run natively when the architecture matches (x86-64), but the operating system and graphics have to be re-implemented and translated.
- **Graphics APIs:** how console GPU commands and shaders are translated to Vulkan, and why shader compilation causes first-time stutter.
- **File and folder structure:** emulators depend on exact names and paths, so getting the Title ID folders right matters.
- **Reading logs to debug:** the log file shows exactly where something fails, which is faster than guessing.
- **Legal and ethical boundaries:** using only my own game and hardware dumps, and keeping copyrighted files out of a public repository.

---

*Author: [nachtlos1](https://github.com/nachtlos1) · CompTIA A+ · CompTIA Network+*
