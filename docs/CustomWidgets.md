# Custom widgets for GlassBar

*Deutsche Fassung: [EigeneWidgets.md](EigeneWidgets.md). Setting names are quoted as they appear in the German UI; with
the English UI they show in English.*

Custom widgets need no C# and no rebuild of GlassBar. A widget is a folder with a `widget.json` that tells GlassBar
whether the data comes from a script, a URL or a file. GlassBar shows the result as a module in the dock and as a card in
the widget panel.

## In 3 steps

1. **Settings → Widgets → custom widgets → "Vorlage anlegen"** (create template) and pick a template: *drives*
   (PowerShell), *countdown* (Python), *web text* (URL) or *status file* (file) – to copy from and adapt.
2. After a moment the widget shows up in the list by itself – GlassBar watches the widgets folder.
3. **Switch the widget on** and place it in the dock under *Ansichten & Module* (views & modules).

The widgets folder is `%AppData%\GlassBar\widgets\` (button "Widget-Ordner öffnen"). Every widget gets its own
subfolder:

```
%AppData%\GlassBar\widgets\
├─ drives\
│  ├─ widget.json
│  └─ drives.ps1
└─ my-widget\
   ├─ widget.json
   └─ status.py
```

GlassBar reloads new, changed or deleted `widget.json` files automatically (short notice above the dock); "Neu laden"
(reload) in the settings does the same by hand. Changes to the script itself take effect on the next refresh.

## widget.json

```json
{
    "id": "co2",
    "title": "CO₂ in the office",
    "glyph": "E9CA",
    "description": "Air quality from the sensor in the hall.",
    "refreshSeconds": 60,
    "source": {
        "type": "command",
        "command": "python",
        "args": ["co2.py"]
    }
}
```

| Field | Required | Meaning |
|---|---|---|
| `title` | no | Name in the dock tooltip, in the panel and in the settings (default: folder name) |
| `id` | no | Unique name (default: folder name) |
| `glyph` | no | Icon: a four-digit code from *Segoe Fluent Icons* (e.g. `E9CA`) or any character/emoji |
| `description` | no | Description in the settings |
| `refreshSeconds` | no | Time between two refreshes, 5 to 86400 seconds (default 60) |
| `source` | **yes** | Where the data comes from – see below |

### Settings fields

With `settings` a widget gets its own fields under its switch in the settings – so you can change e.g. a city or a
server address without touching the script:

```json
"settings": [
    { "key": "city", "label": "City", "type": "text", "default": "Iserlohn" },
    { "key": "limit", "label": "Threshold", "type": "number", "default": "80" },
    { "key": "details", "label": "Show details", "type": "toggle", "default": "true" },
    { "key": "unit", "label": "Unit", "type": "choice", "options": ["°C", "°F"], "default": "°C" }
]
```

| Field | Meaning |
|---|---|
| `key` | Name made of letters, digits and `_` |
| `label`, `description` | Label and help text in the settings |
| `type` | `text`, `number`, `toggle` (value `true`/`false`) or `choice` (with `options`) |
| `default` | Value while nothing has been entered |

The values arrive in two ways: as the **environment variable** `GLASSBAR_<KEY>` (e.g. `$env:GLASSBAR_CITY` in
PowerShell, `os.environ["GLASSBAR_CITY"]` in Python) and as the **placeholder** `{key}` in `args`, `url` and `path` – URL-
encoded automatically in addresses, e.g. `"url": "https://wttr.in/{city}?format=j1"`. A change in the settings refreshes
the widget at once.

### Sources

| `type` | Fields | What happens |
|---|---|---|
| `command` | `command`, `args` | Starts a program in the widget folder (`powershell`, `python`, `node`, an `.exe` next to the widget.json …) and reads its output. At most 15 seconds, without a window. |
| `url` | `url` | Loads `http://` or `https://` – e.g. a status page of your NAS or home server. |
| `file` | `path` | Reads a file (relative to the widget folder or with a full path) that another program writes. |

## What the widget outputs

The simplest is **plain text**: the first line appears in the dock, the rest as the description in the panel.

**JSON** can do more – all fields are optional:

```json
{
    "dockText": "612 ppm",
    "tooltip": "CO₂: 612 ppm – good",
    "value": "612",
    "unit": "ppm",
    "caption": "Air quality good",
    "progress": 0.31,
    "alert": false,
    "history": [580, 590, 602, 612],
    "items": [
        { "label": "Temperature", "value": "22.4 °C", "icon": "E9CA" },
        { "label": "Dashboard", "value": "open", "open": "http://nas.local:3000" }
    ],
    "actions": [
        { "label": "Airing reminder", "open": "ms-settings:notifications" }
    ]
}
```

| Field | Shown as |
|---|---|
| `dockText` | Text next to the icon in the dock (default: `value`) |
| `tooltip` | Tooltip in the dock |
| `value`, `unit` | Big number with unit at the top of the panel |
| `caption` | Sentence under the number |
| `progress` | Bar from 0 to 1 |
| `alert` | `true` colours the dock icon as an alert |
| `history` | Series of numbers as a history curve (up to 120 values) |
| `items` | List with `label`, `value`, optionally `icon` and `open` (up to 12 entries) |
| `actions` | Buttons with `label` and `open` (up to 4) |

`open` may be a web address (`http`/`https`) or a Windows settings page (`ms-settings:…`). Widgets with a command or a
file as source may also open folders (e.g. `C:\`) and files inside their own widget folder (relative paths, no programs
or scripts, no network paths). For safety, widgets with a URL as source only open web addresses and settings pages – a
foreign page must never be able to start a program.

## Examples

**PowerShell** – the first line matters, otherwise umlauts and other non-ASCII characters arrive garbled:

```powershell
[Console]::OutputEncoding = [Text.Encoding]::UTF8
$cpu = (Get-CimInstance Win32_Processor | Measure-Object LoadPercentage -Average).Average
@{ dockText = "$cpu %"; value = "$cpu"; unit = "% CPU"; progress = $cpu / 100 } | ConvertTo-Json -Compress
```

`widget.json`: `"source": { "type": "command", "command": "powershell", "args": ["-NoProfile", "-ExecutionPolicy", "Bypass", "-File", "cpu.ps1"] }`

**Python:**

```python
import json, shutil
total, used, free = shutil.disk_usage("C:\\")
print(json.dumps({"dockText": f"{free // 2**30} GB", "value": str(free // 2**30), "unit": "GB free", "progress": used / total}))
```

`widget.json`: `"source": { "type": "command", "command": "python", "args": ["disk.py"] }`

**URL** (e.g. Uptime Kuma, Home Assistant or your own endpoint that returns the JSON above):

`widget.json`: `"source": { "type": "url", "url": "http://homeserver.local:8123/glassbar.json" }`

## Good to know

- A widget only runs while it is switched on, and like all widgets it pauses during fullscreen games and videos.
- Errors (script not found, timeout, broken JSON) are shown in the widget panel; a broken widget does not slow down the others.
- *Settings → Widgets → custom widgets* shows errors in a `widget.json` as soon as the file is saved.
- GlassBar only runs what is in your widgets folder – only put scripts there that you trust.
