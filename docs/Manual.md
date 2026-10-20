# GlassBar – Manual

*Deutsche Fassung: [Handbuch.md](Handbuch.md). Setting names are quoted as they appear in the German UI; with the
English UI (Settings → System → language) they show in English.*

A floating dock in a graphite look that replaces the Windows 11 taskbar. The accent colour comes from the album cover.

## Build & start

```
dotnet build src/GlassBar/GlassBar.csproj
src/GlassBar/bin/Debug/net10.0-windows10.0.19041.0/GlassBar.exe
```

Quit (the taskbar comes back): tray icon → "Quit GlassBar", the gear in quick access or `GlassBar.exe --exit`.

## Installation (current user only, without admin rights)

```
powershell -ExecutionPolicy Bypass -File scripts\Install.ps1
```

Publishes a release build to `%LOCALAPPDATA%\Programs\GlassBar` and creates a Start menu entry "GlassBar". Autostart
stays off (Settings → System). Running it again updates the installation.

### Uninstall

```
powershell -ExecutionPolicy Bypass -File scripts\Uninstall.ps1
```

Quits GlassBar (the taskbar comes back), removes the program folder, the Start menu entry and an autostart entry. With
`-RemoveData` the settings and caches in `%AppData%\GlassBar` are deleted too. Claude Code hooks in
`%USERPROFILE%\.claude\settings.json` stay untouched – please remove the GlassBar entries there yourself.

## Controls

| Action | How |
|---|---|
| Switch view (apps / spaces / player / custom) | Mouse wheel or touchpad swipe over the dock centre, or click the tabs |
| Open a panel | Click the active tab again, click the cover, launcher button |
| Apps panel | Ctrl+Alt+D (in the panel: Ctrl+K = search, ↑/↓/Enter) |
| Player | Ctrl+Alt+P |
| Briefly show the original taskbar | Ctrl+Alt+B |
| Apply a space | Ctrl+Alt+1–9 |
| Volume | Mouse wheel over the status icons or the speaker in the mini player |
| Quick actions | Right-click the dock (settings, screen edge, auto-hide, show taskbar, pause, power, quit) |
| Calendar | Click the clock |
| Add an app to a custom view | Right-click an app icon → "Ansichten" |
| Claude: new session with a prompt (Claude module on) | Ctrl+Alt+C |
| Claude approval by keyboard (optional) | Ctrl+Enter allow, Ctrl+Backspace deny (selected request only, Claude panel active) |

If a hotkey is already taken by another app, Settings → Bedienung → Tastenkürzel shows it.

## Settings

Gear in quick access → "Settings …" or tray icon → "Settings …".
Look, position, views, controls, media, screens and system behaviour are set here.
"Dock pausieren" (pause dock) brings back the Windows taskbar for a while.

## Look & position

- Display: dock, notch (a narrow pill at the top centre that expands on hover with music, live lyrics, weather, event, timer, Claude and apps) or both – right on the settings overview, under "Position & Größe" or via right-click on the dock.
- Compact dock (default): about 20 % smaller, tabs only as icons. The dock height follows the icon size, and so does the reserved space.
- Font: Manrope (embedded, SIL OFL) or Segoe UI. One switch toggles between Dark and White Matte.
- Colour scheme: dark (graphite), light or "Wie Windows" (follows the Windows app mode live).
- Screen edge: bottom, top, left or right – also different per screen (Bildschirme → "Eigene Position"). At the side the dock shows the apps vertically; spaces and player open as a panel. "Left/right" in the alignment then means top/bottom.
- Fine tuning: UI size (85–130 %), glow (off/subtle/strong/neon), high contrast, opacity, icon size. Changes are visible at once in the preview and the dock; "Abbrechen" restores the old state.
- Modules (clock, system, calendar, timer, weather, Claude) can be placed on the left, in the centre or on the right and reordered.
- Custom views: extra tabs with their own app group (Ansichten & Module → Eigene Ansichten).
- Auto-hide, optionally as a rest capsule (a small pill with the time and the playing title).
- Hiding in fullscreen also detects games in borderless windowed mode.
- Changing texts (Claude status, timer, weather) do not make the dock jump: it grows at once and only glides to a smaller width after 20 seconds.

### Preview in the settings

All pages that concern the look (Aussehen, Design-Studio, Position & Größe, Ansichten & Module, Widgets, Meldungen, Medien) have a live preview on the right: a mini dock with your colours, corners, frame, shadow, font, accent, glow, icon size and spacing, position at the screen edge and the switched-on views and modules – plus whatever fits the page (panel card, notification, player, widget, notch). It stays put while scrolling and shows every change at once, even before the real dock follows. On the design pages, "Vorher · Nachher" (before · after) compares with the saved look; design changes only apply with "Übernehmen". In narrow windows the preview slides over the settings as a smaller strip.

### Design studio

- 17 presets, split into dark (Graphit, Mitternacht, Nord, Sakura, Terminal, Abendrot (formerly "Sonnenuntergang"), Tiefsee, Moos, Espresso, Aurora, Kontrast) and light (Kreide, Papier, Lavendel, Gletscher, Rosé, Salbei). A preset sets all values and the accent colour; after that everything can be adjusted.
- Colours: glass, text, surfaces, edges and optionally a **second accent colour** – sliders, switches, dock markers, progress bars, visualizer and light edges then run as a gradient.
- Shape & frame: corners (round/small/square), **frame** (none/fine/strong) and edge strength.
- Dock: **icon spacing** (2–20 px) and **dock height** (padding around the icons).
- Effects: shadow, **accent shimmer** (how strongly the accent glows in the lower glass) and grain.
- "Exportieren" saves all values including the accent as JSON; "Design als Code" copies one line (GB1:…) to paste on another PC. Older design files without the new values still load with the previous defaults.

## Live activities (notch)

What is running right now is shown by the collapsed notch left of the clock: an icon in a thin progress ring plus a short text, with several activities a counter ("+2"). Expanded, all activities stand as small tiles with progress bars above the cards; a click opens the matching place (downloads folder or finished file in Explorer, start/pause timer, Claude panel, pull request). Finished items briefly get a green check mark (with a small hop, except with "Bewegung reduzieren") and disappear after about 4 seconds.

- Downloads: `.crdownload`, `.part`, `.partial`, `.download`, `.opdownload` in the downloads folder; `.tmp` only once the file grows. Browsers do not reveal the total size, so mostly size and speed instead of percent. Only reads names and sizes, nothing is sent.
- Timers: focus timer and Pomodoro (if switched on) with the remaining time; paused timers stay visible, dimmed.
- Claude Code: "working …" or "waiting for approval" per session, then briefly "done".
- GitHub CI: running checks of your own PRs from the data of the GitHub widget (no own requests; so as current as the widget, every 5 minutes).

Every source can be switched off under Settings → Position & Größe → Live-Aktivitäten. "Auch im Dock zeigen" (off by default) puts a small button with the most important activity into the dock; it can be placed like other modules. A one-second tick only runs while a download grows or a check mark fades out.

## Widgets (Settings → Widgets)

All off by default; clicking the widget in the dock opens its view, position under "Ansichten & Module".

- Local: system monitor (CPU/GPU/RAM), network rate, clipboard (in memory only, password managers are skipped), Pomodoro, notes/to-do (`%AppData%\GlassBar\notes.json`), downloads (drag into other apps), recycle bin, microphone & camera, device battery (Bluetooth, controllers), world clock.
- With internet, only after switching on: GitHub (via the signed-in GitHub CLI `gh`, no token in GlassBar), services (own URLs or `host:port`, checked every 5 minutes), prices (CoinGecko, Stooq).
- Notifications (local): reads the Windows notifications (in memory only), grouped by app with time; "dismiss" removes one, "clear all" empties the notification center, a click brings the app to the front. The count in the dock lights up for new notifications; apps in the dock get a small badge with their count – only when the match is certain (AUMID, package, program path or unique name). The first time it is switched on, Windows asks for access; if denied, "Windows-Einstellungen öffnen" opens Privacy → Notifications. Depending on the Windows version, Windows only hands notifications to Store apps – then the widget says so instead of showing something. Refreshes every 4 seconds while switched on (not during a fullscreen game).
- Own widgets from a script, URL or file: see [CustomWidgets.md](CustomWidgets.md).

### Screenshots

- "Bereich" (region; or Ctrl+Alt+S, changeable under hotkeys, only active with the widget on): a still image of all screens appears dimmed, drag a rectangle and release to take it; Enter takes the whole screen under the mouse pointer, Esc or right-click cancels.
- "Bildschirm" (screen): the monitor under the mouse pointer. "Fenster" (window): the topmost app window you last worked in (without GlassBar).
- Saved as PNG in `Pictures\Screenshots\GlassBar` (the Windows screenshot folder that Win+Print also saves to – follows a moved or OneDrive Pictures folder; the own subfolder keeps the history free of other captures) and also in the clipboard. If Windows blocks the folder (Controlled folder access), the shot goes to `%LocalAppData%\GlassBar\Screenshots`.
- The last 8 captures as previews: a click opens, dragging drops the file into other apps, "Text erkennen" reads the text with Windows text recognition (local, without internet) and copies it. "Bereich → Text" captures and recognises directly, without saving.
- Text recognition needs a Windows language with "Optical character recognition" (Settings → Time & language → Language); if it is missing, the widget shows a hint with a button to the language settings.
- Smart Home (Home Assistant): enter the address (`http://homeassistant.local:8123`), a long-lived access token (Home Assistant → Profile → Security; GlassBar only stores it DPAPI-encrypted for your Windows account) and entities, one per line – `light.kitchen` or `Kitchen | light.kitchen`. "Verbindung testen" queries `/api/`. Lights (on/off, brightness), switches/fans/`input_boolean` (toggle), scenes and scripts ("run"), covers (open/stop/close), sensors and thermostats (display). Refreshes every 30 s and when opened; the dock shows the number of lights on. Only http/https, redirects are not followed.

### Quick notes

- **Ctrl+Alt+N** (changeable under Settings → hotkeys) opens a small input window at the dock from anywhere: type, **Enter** (or Ctrl+Enter) saves, Shift+Enter makes a new line, **Esc** discards. If you just click elsewhere, the draft is kept for the next opening. Also via "Neue Notiz" in the notes widget.
- With the calendar module on, the note can be linked to an event of the next days; an event that is running or starts in less than 30 minutes is preselected.
- The notes widget lists all notes (newest on top, with an event chip) – edit directly, delete, change or remove the link. The to-do list stays below.
- Calendar card (click the clock): "Anstehend" shows the next events, a note icon marks events with notes; a click shows and edits them or adds a new one.
- Five minutes before an event with a note, a notification with the note appears (only while GlassBar runs); the notch shows a note hint for the next event.
- Matching: first by the event's UID, otherwise by start and title – a moved event or a new UID after exporting keeps its notes. Recurring events are still not expanded.
- Everything stays local in `notes.json`; the former single note becomes the first note on first start.

## Checking the design (developers)

```
GlassBar.exe --render-shell <folder> [light]
```

Debug build only: renders dock and notch (collapsed, notification, expanded) as PNG without showing the real windows. Complements `--render-settings` (all pages at the default size 1340 × 900, plus the design studio in a narrow window) and `--render-panels`. Binding errors go to `binding-errors.txt`.

## Tests

```
dotnet run --project tests/GlassBar.Tests
```

Checks calculator, search, module arrangement, widget settings and more, and builds the settings window invisibly (read-only). Exit code = number of failures.

## Updates

Settings, pins, spaces and notes live in `%AppData%\GlassBar` – separate from the program folder, an update only replaces the program. Before the first start of a new version GlassBar makes a backup in `%AppData%\GlassBar\backups`.

**Without an account:** with Settings → System → automatic update check, GlassBar looks at the public downloads page [ahi3000/GlassBar-Releases](https://github.com/ahi3000/GlassBar-Releases) every 6 hours and installs a new version with one click. Every package has a `.sha256` checksum, which the update checks before installing; the package type (with or without bundled .NET) stays the same.

**Via GitHub:** every new version on `main` automatically becomes a release with finished packages. If the downloads page cannot be reached, GlassBar also checks there via the signed-in GitHub CLI `gh` (no token in GlassBar).

`scripts\Install.ps1` remembers the project folder. The installed version checks locally whether a newer version is there and reports it; "Jetzt aktualisieren" under Settings → System runs the script and restarts GlassBar. Nothing goes to the network.

**Moving to another PC:** Settings → System → "Einrichtung übertragen" exports the whole setup without tokens; import it there in the first-run assistant or under the same entry.

## Profiles

Profiles save look, position, views, controls and media (areas selectable one by one) and can be duplicated, exported and imported. Optionally they switch automatically – on battery, on the charger, with one or several screens – but only on a state change.

## Conversions in the search

In the apps panel (Ctrl+Alt+D, Ctrl+K) the search converts units besides calculations: number, unit, "in"/"to"/"nach"/"=", target unit – e.g. "5 km in mi", "12,5 kg to lb", "100 °F in °C", "3 h in min", "1,5 GB in MB" (KB = 1000, KiB = 1024 bytes), "20 m/s in km/h", "2 l in ml". Length, mass, temperature, volume, area, speed, time and data sizes, German and English names, comma or dot as the decimal separator. This always runs offline; Enter copies the number.

Currencies ("100 eur in usd", "50 $ in €") only after Settings → Bedienung → "Währungen umrechnen" (default: off). Then GlassBar loads the ECB reference rates (no account) at most every 12 hours into `%AppData%\GlassBar\currency-rates.xml`; without network the last state applies, the rate date is shown under the result.

## Lyrics (LRCLIB, optional)

Off by default. Switch on right in the player (button "Aa" → "Songtexte aktivieren") or under Settings → Medien. Only then are title, artist and length of the playing song sent to `lrclib.net`; without an artist nothing is searched. The current line then also appears in the dock's mini player and in the notch. Results are kept in `%AppData%\GlassBar\lyrics` and can be removed with "Leeren".

## Claude Code (optional)

1. Settings → Claude Code → switch the module on and apply.
2. Copy the configuration shown and paste it yourself into `%USERPROFILE%\.claude\settings.json` under `hooks` – do not overwrite existing hooks, add to them.
3. Optionally enable "Freigaben im Dock beantworten" (answer approvals in the dock). On timeout, connection error or when GlassBar is not running, nothing is ever allowed automatically – then the terminal asks as usual.

After installing with the script, copy the configuration again, as the path to `GlassBar.Hook.exe` changes. WSL is not supported.

**Tokens & costs:** the Claude card reads the local transcripts under `%USERPROFILE%\.claude\projects` (only while the card is open, only newly appended lines) and shows tokens and approximate cost per running session, below that "Letzte Sitzungen" (a click opens the project folder) and today's total. The costs are an estimate from the public API list prices without guarantee – with a Claude subscription you do not pay per token. For an unknown model it shows "–".

**Quick prompt:** Ctrl+Alt+C (changeable under Bedienung → Tastenkürzel) or "Neue Sitzung" in the Claude card opens a small input window: pick a project folder (recently used projects or "Ordner wählen …"), enter a prompt, Enter starts `claude` in a new Windows Terminal tab (without Windows Terminal in a console window). The prompt is never passed through a command line or shell, but through a private temp file to `GlassBar.Hook.exe`, which starts `claude` directly. After quitting, a PowerShell stays open in the folder. Needs `claude.exe` (native installer) or an npm installation with Node.

## Spotify (queue + favourite)

1. Create an app at https://developer.spotify.com/dashboard.
2. Enter `http://127.0.0.1:47812/callback` as the redirect URI.
3. Paste the client ID under Settings → Medien and click "Verbinden".

The queue can only be read via the API; "play now" needs Spotify Premium.

## Tray

The arrow in the dock shows the icons of other programs (Discord, Steam …). Clicks go to the program like with the Windows taskbar, a middle click pins an icon straight into the dock. Technically GlassBar registers as an additional taskbar window and passes every message on to the Windows taskbar – so "show original taskbar" still has all icons. Switch off: Settings → Ansichten & Module → Infobereich.

## Bluetooth devices

The quick settings list paired devices. Headphones/headsets connect and disconnect there with a button – GlassBar uses the same switch of the Bluetooth audio driver as the Windows settings. Other devices (controller, mouse) show status and battery. Read-only check: `dotnet run --project tests/GlassBar.Tests -- --probe-bluetooth`.

## Windows key (optional)

Settings → Bedienung → "Windows-Taste öffnet das Apps-Panel". A keyboard hook on its own thread detects the Win key pressed alone; on release GlassBar sends an unassigned key code (0xE8) before the release so that Windows does not open the Start menu (same method as PowerToys). Shortcuts with Win stay unchanged, Ctrl+Esc opens the Start menu.

## Audio visualizer

Settings → Medien → "Audio-Visualizer". GlassBar listens locally via WASAPI loopback to what the speakers play and calculates 24 frequency bands (nothing is stored or sent). Runs only during playback, pauses in fullscreen and with "Bewegung reduzieren". Check without GlassBar: `dotnet run --project tests/GlassBar.Tests -- --probe-visualizer`.

## Power menu

Right-click the dock → "Ein/Aus": lock, sleep, sign out, restart and shut down, like the Start menu. Sign out, restart and shut down ask first; without hibernation, shut down falls back to a normal shutdown.

## Crash protection

A small watchdog process starts with GlassBar (`GlassBar.exe --watchdog`, without a window). If GlassBar ends unexpectedly, it brings back the Windows taskbar, writes `crash.log` and restarts GlassBar – at most 3 times in 10 minutes. A normal exit, Windows shutting down, Task Manager and `Stop-Process` do not trigger a restart.

## Data

- Error log `error.log` only when Settings → System → "Lokale Diagnose" is on (default: off). "Fehlerbericht erstellen" packs logs and settings without tokens and without your Windows user name into a zip; nothing is sent.
- Settings: `%AppData%\GlassBar\settings.json` (pins, spaces, look). The Spotify token is DPAPI-encrypted.
- Space previews: `%AppData%\GlassBar\spaces\*.png`
- Caches: `weather-cache.json`, `lyrics\`, `currency-rates.xml` (only when the modules are active)
- Crash log `crash.log` (always, small, local only) and `crash-history.txt` for the restart limit
- Pinned clipboard entries: `clipboard-pins.json` and images in `clipboard-pins\`

## If the taskbar is missing after a crash

Run `GlassBar.exe --restore-taskbar` (with the setup or install script: Start menu → "GlassBar - Restore taskbar"). This also works when GlassBar has crashed. Alternatively restart Explorer in Task Manager.

## If the dock looks empty (only glass, no icons)

Then Windows' graphics acceleration hangs (affects all WPF programs, not just GlassBar). Restart the graphics driver with Win+Ctrl+Shift+B or restart the PC. If that does not help for good: switch on Settings → System → "Software-Rendering" and restart GlassBar.

## Language

German and English. Settings → System → language: "Wie Windows", "Deutsch" or "English" (applies after restart).
New UI texts: write them in German, in C# with `Loc.T("…")`/`Loc.F("… {0}", value)`, in XAML with `{loc:Tr '…'}`, and add the English translation to `src/GlassBar/Localization/en.json` – a test checks that none is missing.
