# Eigene Widgets für GlassBar

*English version: [CustomWidgets.md](CustomWidgets.md).*

Eigene Widgets brauchen kein C# und kein Neubauen von GlassBar. Ein Widget ist ein Ordner mit einer `widget.json`, die einem
Skript, einer URL oder einer Datei sagt, woher die Daten kommen. GlassBar zeigt das Ergebnis als Modul im Dock und als
Karte im Widget-Panel.

## In 3 Schritten

1. **Einstellungen → Widgets → Eigene Widgets → „Vorlage anlegen“** und eine Vorlage wählen: *Laufwerke* (PowerShell),
   *Countdown* (Python), *Web-Text* (URL) oder *Status-Datei* (Datei) – zum Abschauen und Anpassen.
2. Das Widget erscheint nach einem Moment von selbst in der Liste – GlassBar beobachtet den Widget-Ordner.
3. **Widget einschalten** und unter *Ansichten & Module* im Dock platzieren.

Der Widget-Ordner liegt unter `%AppData%\GlassBar\widgets\` (Knopf „Widget-Ordner öffnen“). Jedes Widget bekommt einen
eigenen Unterordner:

```
%AppData%\GlassBar\widgets\
├─ laufwerke\
│  ├─ widget.json
│  └─ laufwerke.ps1
└─ mein-widget\
   ├─ widget.json
   └─ status.py
```

Neue, geänderte oder gelöschte `widget.json` lädt GlassBar automatisch neu (kurze Meldung über dem Dock); „Neu laden“ in
den Einstellungen macht dasselbe von Hand. Änderungen am Skript selbst wirken beim nächsten Aktualisieren.

## widget.json

```json
{
    "id": "co2",
    "title": "CO₂ im Büro",
    "glyph": "E9CA",
    "description": "Luftqualität vom Sensor im Flur.",
    "refreshSeconds": 60,
    "source": {
        "type": "command",
        "command": "python",
        "args": ["co2.py"]
    }
}
```

| Feld | Pflicht | Bedeutung |
|---|---|---|
| `title` | nein | Name im Dock-Tooltip, im Panel und in den Einstellungen (Standard: Ordnername) |
| `id` | nein | Eindeutiger Name (Standard: Ordnername) |
| `glyph` | nein | Symbol: vierstelliger Code aus *Segoe Fluent Icons* (z. B. `E9CA`) oder ein beliebiges Zeichen/Emoji |
| `description` | nein | Beschreibung in den Einstellungen |
| `refreshSeconds` | nein | Abstand zwischen zwei Aktualisierungen, 5 bis 86400 Sekunden (Standard 60) |
| `source` | **ja** | Woher die Daten kommen – siehe unten |

### Einstellungsfelder

Mit `settings` bekommt ein Widget eigene Felder unter seinem Schalter in den Einstellungen – so lässt sich z. B. eine
Stadt oder Server-Adresse ändern, ohne das Skript anzufassen:

```json
"settings": [
    { "key": "city", "label": "Stadt", "type": "text", "default": "Iserlohn" },
    { "key": "limit", "label": "Grenzwert", "type": "number", "default": "80" },
    { "key": "details", "label": "Details zeigen", "type": "toggle", "default": "true" },
    { "key": "unit", "label": "Einheit", "type": "choice", "options": ["°C", "°F"], "default": "°C" }
]
```

| Feld | Bedeutung |
|---|---|
| `key` | Name aus Buchstaben, Ziffern und `_` |
| `label`, `description` | Beschriftung und Hilfetext in den Einstellungen |
| `type` | `text`, `number`, `toggle` (Wert `true`/`false`) oder `choice` (mit `options`) |
| `default` | Wert, solange nichts eingetragen ist |

Die Werte kommen auf zwei Wegen an: als **Umgebungsvariable** `GLASSBAR_<KEY>` (z. B. `$env:GLASSBAR_CITY` in PowerShell,
`os.environ["GLASSBAR_CITY"]` in Python) und als **Platzhalter** `{key}` in `args`, `url` und `path` – in Adressen
automatisch URL-kodiert, z. B. `"url": "https://wttr.in/{city}?format=j1"`. Eine Änderung in den Einstellungen
aktualisiert das Widget sofort.

### Quellen

| `type` | Felder | Was passiert |
|---|---|---|
| `command` | `command`, `args` | Startet ein Programm im Widget-Ordner (`powershell`, `python`, `node`, eine `.exe` neben der widget.json …) und liest seine Ausgabe. Höchstens 15 Sekunden, ohne Fenster. |
| `url` | `url` | Lädt `http://` oder `https://` – z. B. eine Status-Seite vom NAS oder Homeserver. |
| `file` | `path` | Liest eine Datei (relativ zum Widget-Ordner oder mit vollem Pfad), die ein anderes Programm schreibt. |

## Was das Widget ausgibt

Am einfachsten ist **reiner Text**: die erste Zeile erscheint im Dock, der Rest als Beschreibung im Panel.

Mehr geht mit **JSON** – alle Felder sind freiwillig:

```json
{
    "dockText": "612 ppm",
    "tooltip": "CO₂: 612 ppm – gut",
    "value": "612",
    "unit": "ppm",
    "caption": "Luftqualität gut",
    "progress": 0.31,
    "alert": false,
    "history": [580, 590, 602, 612],
    "items": [
        { "label": "Temperatur", "value": "22,4 °C", "icon": "E9CA" },
        { "label": "Dashboard", "value": "öffnen", "open": "http://nas.local:3000" }
    ],
    "actions": [
        { "label": "Lüften-Erinnerung", "open": "ms-settings:notifications" }
    ]
}
```

| Feld | Anzeige |
|---|---|
| `dockText` | Text neben dem Symbol im Dock (Standard: `value`) |
| `tooltip` | Tooltip im Dock |
| `value`, `unit` | Große Zahl mit Einheit oben im Panel |
| `caption` | Satz unter der Zahl |
| `progress` | Balken von 0 bis 1 |
| `alert` | `true` färbt das Dock-Symbol als Hinweis |
| `history` | Zahlenreihe als Verlaufskurve (bis zu 120 Werte) |
| `items` | Liste mit `label`, `value`, optional `icon` und `open` (bis zu 12 Einträge) |
| `actions` | Knöpfe mit `label` und `open` (bis zu 4) |

`open` darf eine Web-Adresse (`http`/`https`) oder eine Windows-Einstellungsseite (`ms-settings:…`) sein. Widgets mit
Befehl oder Datei als Quelle dürfen zusätzlich Ordner öffnen (z. B. `C:\`) und Dateien innerhalb ihres Widget-Ordners
(relative Pfade, keine Programme oder Skripte, keine Netzwerkpfade). Widgets mit URL als Quelle öffnen aus Sicherheitsgründen nur Web-Adressen und Einstellungsseiten – eine
fremde Seite soll nie ein Programm starten können.

## Beispiele

**PowerShell** – wichtig ist die erste Zeile, sonst kommen Umlaute verstümmelt an:

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
print(json.dumps({"dockText": f"{free // 2**30} GB", "value": str(free // 2**30), "unit": "GB frei", "progress": used / total}))
```

`widget.json`: `"source": { "type": "command", "command": "python", "args": ["disk.py"] }`

**URL** (z. B. Uptime-Kuma, Home Assistant oder ein eigener Endpunkt, der das JSON oben liefert):

`widget.json`: `"source": { "type": "url", "url": "http://homeserver.local:8123/glassbar.json" }`

## Gut zu wissen

- Ein Widget läuft nur, solange es eingeschaltet ist, und pausiert wie alle Widgets bei Vollbild-Spielen und -Videos.
- Fehler (Skript nicht gefunden, Zeitüberschreitung, kaputtes JSON) stehen im Widget-Panel; ein kaputtes Widget bremst die anderen nicht.
- Fehler in einer `widget.json` zeigt *Einstellungen → Widgets → Eigene Widgets* an, sobald die Datei gespeichert ist.
- GlassBar führt nur aus, was in deinem Widget-Ordner steht – lege dort nur Skripte ab, denen du vertraust.
