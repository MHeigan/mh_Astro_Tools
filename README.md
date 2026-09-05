# mh_astro_tools Suite — v2.0.0

Night-vision (red-on-black) utilities for astrophotography and astronomy. One
Launcher, six tools, all sharing a dark theme that protects dark adaptation at
the telescope.

© 2026 Martin P. Heigan · [anti-matter-3d.com](https://anti-matter-3d.com) ·
licensed under 
[CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

## What's new in v2.0.0

Suite-wide migration from Python 3.8 / Tkinter (and PyQt5/PyQt6) to **Python 3.13
/ PySide6 (Qt6)**, on one consistent red-on-black night-vision design: a unified
theme, a deterministic red text caret across all input fields, per-user
writable config under `Documents\\mh_astro_tools\\`, wheel-safe dropdowns,
frameless title-bar chrome and a fade-from-black on launch. See `RELEASE.md` for
the full notes.

## Tools


|Tool                 |Description                                                                                                                                      |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
|**Suite Launcher**   |Opens every tool; Full and Compact modes; right-click any tool for its PDF manual.                                                               |
|**Exposure Calculator**|Deep-sky sub-exposure planner — AB-magnitude SNR model, field of view, pixel scale, sky-limited time, limiting magnitude.                        |
|**UTC / GPS / Weather**|Local time, UTC, exposure timer, saved GPS site, and an auto-refreshing astronomy + weather panel (sun/moon, dew, pressure, seeing, declination).|
|**500 Rule Calculator**|Maximum tripod exposure before star trailing: 500 / (focal × crop).                                                                              |
|**Night Vision Overlay**|Adjustable red screen tint — click-through, all monitors, global hotkeys, presets, region cutout.                                                |
|**Scientific Calculator**|Night-vision scientific / regular calculator with DEG/RAD trig, a safe expression evaluator, and a memory panel.                                 |
|**Weather Widget**   |Compact desktop weather HUD with a condensation-risk readout, living in the system tray.                                                         |

## Download


[mh_Astro_Tools_Suite_Win_Setup_v2_0_0.exe](https://github.com/MHeigan/mh_Astro_Tools/releases/tag/mh_astro_tools_v2_0_0)
— GitHub release (recommended)

Also available direct from 
[anti-matter-3d.com](https://anti-matter-3d.com/mh_tools/mh_astro_tools/mh_Astro_Tools_Suite_Win_Setup_v2_0_0.exe)
, and listed on the [Tools page](https://anti-matter-3d.com/tools/).

SHA-256:

```
26d3242d1e6ea2019a240ada110464525b444b19c7a10a14cd6dfb910dd88f29
certutil -hashfile mh_Astro_Tools_Suite_Win_Setup_v2_0_0.exe SHA256
```
## Distribution

Installed by a single Windows installer. No Python or runtime is required —
everything is bundled. User manuals (one PDF per tool plus a START HERE guide)
and `License_Agreement.pdf` are installed alongside the tools.

## System requirements


|                |                                                                          |
|----------------|--------------------------------------------------------------------------|
|Operating system|Windows 10 / 11, 64-bit                                                   |
|Display         |1920×1080 or larger recommended; the Overlay supports multiple monitors   |
|Internet        |Optional — only UTC / GPS / Weather and the Weather Widget fetch live data|
|Runtime         |None; Python and all libraries are bundled                                |

User settings, saved locations and exported results live under 
`Documents\\mh_astro_tools\\` and are preserved across upgrades and uninstalls.

## Licence

Licensed under **CC BY-NC-ND 4.0** — see [License.md](License.md) and the
installed `License_Agreement.pdf`. Non-commercial use, attribution required, no
redistribution of modified versions.

## Security

All executables are code-signed by Certum with SHA-256 Authenticode and an
RFC-3161 timestamp, and are submitted to Microsoft WDSI and VirusTotal before
release. Each installed tool folder carries a signed `manifest.cat` and a `
SHA256SUMS.txt`, and the installer's own SHA-256 is published above. Freshly
built Nuitka/MSVC binaries — calculation tools especially — can trigger
heuristic, reputation-based warnings in some browsers and antivirus engines;
these are not malware detections. Verify the Authenticode signature and the
published SHA-256 if in doubt.

## Links

- Tools & updates — <https://anti-matter-3d.com/tools/>
- Contact — <https://anti-matter-3d.com/contact/>
