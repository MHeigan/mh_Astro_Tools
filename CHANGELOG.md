# Changelog — mh_astro_tools suite

All notable changes to the suite are documented here. Format based on
[Keep a Changelog](https://keepachangelog.com/). The suite is licensed under
**CC BY-NC-ND 4.0** — © 2026 Martin P. Heigan.

## [2.0.0] — 2026 (Qt6 / PySide6 rebuild)

Suite-wide migration from Python 3.8 / Tkinter (and PyQt5/PyQt6) to **Python 3.13 /
PySide6 (Qt6)**, on a single consistent red-on-black **night-vision** design.

### Added (suite-wide)
- Unified night-vision theme: Fusion base + red/black `QPalette`, custom red arrow/I-beam
  cursors, frameless title-bar chrome with version badge, taskbar minimise, and a
  fade-from-black on launch.
- Deterministic red text caret across all input fields (replaces the leaking native white
  caret) via an app-wide proxy style + a custom line edit that paints its own caret.
- Writable per-user config/results under `Documents\mh_astro_tools\…` (fixes silent save
  failures when installed to read-only Program Files); legacy locations are read as a
  fallback so existing settings migrate.
- Wheel-safe dropdowns (scrolling the window no longer changes a selection by accident).

### Changed (suite-wide)
- All tools standardised to version **2.0.0**.
- Windows builds via shared-venv Nuitka **and** PyInstaller bats (ASCII/CRLF), EN-US
  VERSIONINFO, AppUserModelID `mh_tools.<tool>.2_0_0`, per-tool icons under `_internal\.assets`.

---

### Exposure Calculator — `2.0.0`
- Ported to PySide6; night-vision standard applied (20 input fields + Notes area now use
  the red-caret line/text edits).
- Profiles (.json) and export (.txt / .csv / .json) retained; calculations remain fully
  offline (no live data fetch).
- **Physics accuracy pass (no version bump — same 2.0.0).** Independently re-derived
  against the AB-magnitude photon-counting model and cross-checked against SharpCap
  (ASI224MC, 135 mm f/3.5, Bortle 6: 8.4–9.0 e-/px/s vs Glover's 9.26).
  - Aperture is now the **native entrance pupil**, `D = F / N_native`. Previously
    `f_eff / N` inflated D under a Barlow and left the sub time unchanged. A Barlow or
    reducer now scales only `f_eff` and `N_eff`, and sky rate goes as `1/N_eff²`, so a
    2× Barlow correctly lengthens the sky-limited sub 4×.
  - **Broadband passband is camera-type aware** — ~100 nm per Bayer channel for colour
    (preserving the ASI224MC validation) vs ~300 nm luminance for mono, which was
    previously under-estimated 3×. Camera Type now participates in the physics rather
    than being cosmetic.
  - **Sky-Limited Sub shows the raw physical value** (to 0.1 s below 60 s); the 15 s
    practicality floor applies to the recommendation only,
    `t_rec = clamp(t_sky, 15 s, cap)`, with Calculation Notes when either bound engages.
  - New **Effective Focal Ratio** results row, flowing into the panel and the
    .txt / .csv / .json exports; added notes and tooltips for Barlow effective f-ratio
    and the broadband passband assumption.
  - Fixed the combo popup **white cursor leak** (pre-existing and suite-wide, not a
    regression from this pass) in `_NoScrollComboBox`.
  - Exports recalculated under a Barlow or a mono broadband configuration will
    legitimately differ from ones produced before this pass.

### UTC / GPS / Weather — `2.0.0`
- Ported to PySide6; large clock now uses an inline-stylesheet font so it renders at full
  size.
- Weather panel enriched: wind, relative humidity, dew **spread** with EXTREME/HIGH/MED/LOW
  risk labels, WMO condition text, MSL-primary pressure (with surface pressure), and moon
  age. Open-Meteo upgraded to the `current=` block. Units toggle (°C/°F, km/h/mph, m/ft).
- Location and saved data live under `Documents\mh_astro_tools`.

### Weather Widget — `2.0.0`
- Ported from PyQt6 to PySide6.
- **Removed the startup "veil"** (the full-desktop black startup cover); startup now relies
  on the frameless window + per-widget fade.
- Config moved to `Documents\mh_astro_tools\mh_astro_weather_widget\` (legacy fallback).
- Fixed a latent crash in the no-location branch (undefined `ts_str`); corrected the About
  box copyright to **© 2026**.
- Native WinRT geolocation is optional (falls back to IP-based location). Added `tzdata` so
  timezone resolution works on Windows. System-tray HUD (intentionally not in the taskbar).

### 500 Rule Calculator — `2.0.0`
- Rebuilt on PySide6/night-vision. Same model: `500 / (focal × crop)` over an 8-sensor table
  (default Full Frame + 24 mm → 20.8 s).
- Live recalculation as the sensor or focal length changes; the bright error dialog is
  replaced by an inline red message.

### Night Vision Calculator — `2.0.0`
- Rebuilt on PySide6/night-vision. Opens in Scientific mode and toggles to Regular with a
  smooth dark crossfade (now ~400 ms); the window size stays constant as the number pad
  expands.
- Safe AST expression evaluator (DEG/RAD trig, `ln` natural / `log` base-10, factorial,
  square root; rejects code execution such as `__import__`).
- Memory list with Use / Copy / Clear; keyboard Enter / Esc / Backspace.
- Fixed uneven keypad spacing in Regular mode (keys now fill their cells) and removed the
  deprecated `ast.Num` node (Python 3.14-safe).

### Night Vision Overlay — `2.0.0`
- Ported from Qt5 to PySide6; the proven click-through (`WS_EX_LAYERED | WS_EX_TRANSPARENT`)
  and always-on-top re-assert logic are preserved.
- **New control dock for maximum flexibility:**
  - Global hotkeys that work even when another app has focus — toggle (Ctrl+Alt+N), tint
    ± (Ctrl+Alt+↑/↓), dim ± (Ctrl+Alt+←/→), and panic instant-off (Ctrl+Alt+0).
  - Decoupled **Dim (black)** and **Tint (colour)** layers, so "how dark" and "how red" are
    independent.
  - **Hue** slider from deep red to amber.
  - Four user presets (one tap to recall; arm **Save**, then tap a slot to store), persisted
    to the settings file.
  - Per-monitor on/off toggles; overlays rebuild automatically when monitors are added or
    removed.
  - Numeric % readouts, arrow-key fine nudge, and a Reset on each control.
  - Collapsible dock that snaps to a screen edge and can auto-hide to a slim handle.
  - **Region cutout** — drag a rectangle to keep one area un-tinted.
- Settings moved to `Documents\mh_astro_tools\mh_night_vision_overlay\` (old keys migrated).
- Note: a true gamma/midtone curve is not offered — an additive translucent overlay can only
  darken/tint what is below it, not remap the underlying framebuffer.

---

### Suite Launcher — `2.0.0`
- **New tool** in the v2 suite — the launcher itself ported from Tkinter to PySide6.
- Frameless red-on-black night-vision window matching the rest of the suite. Splash with
  300 ms fade-in / ~1.9 s hold / 800 ms crossfade into the launcher.
- **Two display modes:**
  - **Full** (default, always at launch) — six wide text-labelled tool buttons in a single row.
  - **Compact** — six 52 × 52 px icon buttons centred in a tight ~367 px wide bar, for keeping
    the launcher visible alongside capture software without taking screen space.
- Compact mode is **runtime-only**; the launcher always opens expanded (intentional — no
  persisted "remember collapsed" surprise).
- **Single-instance:** second launches raise the first via `QLocalServer/QLocalSocket`.
- **Default position:** bottom-centre, 10 px above the taskbar. User can drag during the
  session; not persisted (next launch returns to default).
- **Title-bar chrome:** five outlined buttons — compact toggle (`⧉`), preferences (`≡`),
  about (`i`), minimise, close. Visual centre is preserved across mode toggles so the
  launcher doesn't visually "slide" when its width changes.
- **Right-click any tool button** → "Open User Manual (PDF)". Preferences dialog has an
  "Open Manuals Folder" button.
- **Keyboard:** `1`..`6` launch by position, `Esc` collapses to compact, `Ctrl+Q` quits.
- **Preferences:** transparency 40–100% (default 80%), always-on-top, launch Night Vision
  Overlay on startup, edge-snap, auto-collapse after launching a tool (default off).
- Resolves sibling tool exes from the install layout and greys out missing tools with a
  "Tool not installed" tooltip.
- Settings under `Documents\mh_astro_tools\mh_astro_tools_launcher\settings.json`.
- About-dialog URL displays without the trailing slash (`https://anti-matter-3d.com/tools`)
  while still linking to the canonical URL — consistent with the Weather Widget.
- **Fixed (post-release):** About and Preferences dialogs opened partially off-screen below
  the bottom-docked launcher and couldn't be grabbed (frameless title bar landed under the
  taskbar). They now size to content, centre on the dock, then clamp into the screen work
  area (`availableGeometry`) so they always open fully visible above the taskbar — in both
  full and compact modes.

---

### Inno Setup Installer — `2.0.0`
- New installer for the v2 layout, compiles on **Inno Setup 6.7.0**. Flat per-tool source
  folders in the release root map into the nested install layout under
  `{app}\mh_Astro_Tools_suite\`. **AppId preserved** for a clean in-place upgrade from 1.x.
- **Modern dark slate** wizard theme (`WizardStyle=modern dark`, background BGR `$332B26` =
  RGB `#262B33`) — the mh_tools default going forward; per-tool accent via the red banner art.
- **Three tasks, all on by default:** Start Menu shortcuts, Desktop shortcut, and "Create my
  settings folder" (`Documents\mh_astro_tools`). Launcher shortcuts use the rocket
  `mh_Astro_Tools_Launcher.ico`; Start Menu **and** Desktop both get a Manuals-folder
  shortcut (`manuals.ico`).
- Prior-version cleanup sweeps old Start Menu **and** Desktop shortcuts (current + legacy
  names); `_internal` trees pre-cleared of hidden/system attributes, then re-hidden.
- `build_installer.bat` uses `goto` labels (not parenthesised `if` blocks) to avoid the
  `Program Files (x86)` `)` cmd-parser trap.
- **Installer EXE version info** set natively via `[Setup]` directives (no post-build inject):
  `VersionInfoVersion`/`VersionInfoProductVersion` = `2.0.0.0` (was `File version 0.0.0.0`),
  `VersionInfoCopyright` / `AppCopyright` = `© 2026 Martin P. Heigan` (was blank), plus
  `VersionInfoCompany` (`mh_astro_tools`), `VersionInfoOriginalFileName`, and explicit
  Product name / description. The `.iss` is now **UTF-8 with BOM** (for the `©`). Recompile
  before signing so the signature covers the corrected version info.
- **Themed variant** (`…_themed.iss`): wires the redistributable **RubyGraphite** VCL style
  (from RAD Studio `\Redist\styles\Vcl\`, renamed `mh_astro_theme.vsf` in `_internal\.assets\`)
  via `WizardStyleFile` — red checkbox/button/progress accents, embedded at compile time, no
  `[Files]` entry. **Confirmed working and adopted as the astrophotography-suite installer**
  (outputs the canonical `…_v2_0_0.exe`). A genuine red accent is only possible via a custom
  `.vsf` — under a built-in style, Pascal-script colour properties are ignored and no built-in
  style is red. **Theme policy:** dark slate + blue (no `.vsf`) is the default for new mh_tools
  installers; the RubyGraphite `.vsf` is used for all astrophotography tools.

### User Manuals — `2.0.0`
- Regenerated for all seven v2 tools (`.docx`), plus equation appendices for the Exposure
  Calculator and 500 Rule Calculator. 500 Rule is beginner-level; the rest are technical.

## [Unreleased]

Release-engineering hardening from the 2026-09-04 build audit, opened after
`Microsoft:Trojan:Win32/Wacatac.C!ml` false positives on the Nuitka payload exes and
the Inno Setup EXE, plus detections from Google's engine. Nothing here changes tool
behaviour; it changes how the suite is built, signed and packaged. Folds into `2.0.0`
if that has not shipped, otherwise becomes `2.0.1`.

### Fixed
- **Exposure Calculator `CompanyName` was `mh_vfx_tools`.** Corrected to
  `mh_astro_tools` in both `build_mh_astro_exposure_calculator_v2_0_0_nuitka.bat` and
  `version_mh_astro_exposure_calculator_v2_0_0.txt`. The `.iss` already declared
  `VersionInfoCompany=mh_astro_tools`, so the installer and the payload it wrapped had
  been disagreeing. The other six tools were verified correct.
- **Installer no longer spawns `cmd.exe`.** The `[Code]` upgrade-safety step ran
  `Exec({cmd}, '/c attrib -h -s "{app}\*" /s /d')`, which is behaviourally
  indistinguishable from a dropper unstaging its payload and is a real AV heuristic
  trigger for the installer itself. Replaced with `GetFileAttributesW` /
  `SetFileAttributesW` imported from kernel32 and a recursive `FindFirst`/`FindNext`
  walk. Identical effect, no child process.
- **VERSIONINFO injection could report success after failing.** The injector printed a
  warning when `0x0409` was absent and still exited 0, so the calling bat's
  `if errorlevel 1` check could never fire. It now exits 1, and additionally retries
  through a Defender file lock with backoff, verifies the exe did not shrink (the PE
  overlay being discarded and not re-appended), and restores from the pre-injection
  backup on any failure.
- **Stray `.pre_versioninfo.bak` files could ship.** No build bat deleted the backup on
  success. A surviving `.bak` is a complete unsigned second copy of a payload exe, and
  `[Files]` with `recursesubdirs` would install it on the customer's machine outside
  any catalogue. The bats now delete it on success, keep it on failure, and the
  installer bat hard-fails if one is found anywhere in the payload.

### Added
- **The uninstaller is now signed.** `SignTool=mhsign` + `SignedUninstaller=yes` in
  `[Setup]`. Inno generates `unins000.exe` on the customer's machine at install time, so
  it can never be signed by hand afterwards — without this, every install dropped an
  unsigned executable into `{app}` inside a hidden+system tree. ISCC also signs the
  Setup EXE in the same pass. Requires a one-time `mhsign` entry under
  **Tools → Configure Sign Tools** in the Inno IDE.
- **`build_installer_themed.bat` gained six gates**, all blocking except the last:
  seven payload folders present; `manuals\` present and holding PDFs (it ships with
  `skipifsourcedoesntexist`, so its absence was silent and left the Manuals shortcuts
  opening nothing); no `__pycache__`; no `.pre_versioninfo.bak`; `README.txt` and
  `License.txt` per folder; `verify_installer_payload.py`. `SHA256SUMS.txt` per folder
  warns rather than fails.
- **`INJECT` A/B flag on every Nuitka bat.** `INJECT=0` skips the post-build PE resource
  rewrite, producing a control build for testing whether that step contributes to the ML
  detections. Diagnostic only, never a shipping configuration.
- **`verify_versioninfo_consistency.py`** — cross-checks every build bat against the
  VERSIONINFO `.txt` it injects. The `.txt` overwrites whatever Nuitka set, so a correct
  bat paired with a stale `.txt` ships the stale value silently; this is what let the
  `mh_vfx_tools` string survive. Checks the five identity strings, InternalName and
  OriginalFilename against `APPNAME`, the version tuples, `Translation` being
  `[1033, 1200]`, and ASCII/CRLF. Run with `--expect-company mh_astro_tools`.
- **`mh_Astro_Tools_Suite_Release_Build_Order.md` / `.pdf`** — the fixed eight-step
  order with the reasons each step cannot be reordered, the install-time attribute
  sequence, and the open items.

### Changed
- **`DisableDirPage=no`.** A silent per-user install into LocalAppData with the
  directory page suppressed is the install profile of an adware dropper, and it stacked
  with the hidden+system `[Dirs]`. The default path is unchanged; the user now sees it.
- **Distribution is installer-only.** No portable ZIP, so `hide_for_zip.bat` leaves the
  pipeline entirely. Nothing now hides a payload file at any point, which removes the
  short-installer failure class (ISCC expands `[Files]` wildcards over normal files
  only and skips hidden loose files silently).
- **`__pycache__` is a hard failure, not an auto-clean.** Once `mh_zip_sign_pack_gui`
  has written `SHA256SUMS.txt`, `manifest.json` and the signed `.cat`, any change to the
  payload invalidates all three. A stop reports that the payload moved after hashing;
  an auto-clean would hide it.
- Build bats gained `EnableDelayedExpansion`, source and venv guards, goto-label error
  paths, and a hard gate on the injector and version file being present.
- The `--noinclude-*-mode=nofollow` anti-bloat guards are **deliberately omitted** while
  the AV investigation is open — they shrink the dist, and dist size is one of the
  variables under test. Add them once it closes.

### Notes — certificate (governs the whole suite)
Everything released so far is signed with Certum's **Open Source Code Signing** product,
not the OV certificate the tooling and documentation claim:
`CN="Open Source Developer, Martin Pieter Heigan", O=Open Source Developer`, thumbprint
`5BB5526A8EA0674E00B7EB86CC0943321EA5B0BB`, valid to **2026-11-17**. Certum issue it to
individuals only and revoke it if used to sign commercially distributed software.

Renewal is targeted for early October 2026; an expired certificate cannot be renewed.
Any renewal generates a new key pair and therefore a new thumbprint, so certificate
reputation resets regardless of which product is bought — which makes October the
cheapest moment to move to the commercial Standard (OV) certificate.

Note also that EV certificates no longer grant instant SmartScreen reputation; Microsoft
removed that behaviour in 2024 and no longer recommends paying the premium for it.

[2.0.0]: https://anti-matter-3d.com/
