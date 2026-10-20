# GlassBar – Handbuch

*English version: [Manual.md](Manual.md).*

Ein schwebendes Dock im Graphit-Look als Ersatz für die Windows-11-Taskleiste. Die Akzentfarbe kommt aus dem Albumcover.

## Build & Start

```
dotnet build src/GlassBar/GlassBar.csproj
src/GlassBar/bin/Debug/net10.0-windows10.0.19041.0/GlassBar.exe
```

Beenden (die Taskleiste kommt zurück): Tray-Icon → „GlassBar beenden“, Zahnrad im Schnellzugriff oder `GlassBar.exe --exit`.

## Installation (nur aktueller Benutzer, ohne Adminrechte)

```
powershell -ExecutionPolicy Bypass -File scripts\Install.ps1
```

Veröffentlicht einen Release-Build nach `%LOCALAPPDATA%\Programs\GlassBar` und legt einen Startmenü-Eintrag „GlassBar“ an. Autostart bleibt aus (Einstellungen → System). Ein erneuter Aufruf aktualisiert die Installation.

### Deinstallation

```
powershell -ExecutionPolicy Bypass -File scripts\Uninstall.ps1
```

Beendet GlassBar (Taskleiste kommt zurück), entfernt Programmordner, Startmenü-Eintrag und einen Autostart-Eintrag. Mit `-RemoveData` werden zusätzlich Einstellungen und Caches in `%AppData%\GlassBar` gelöscht. Claude-Code-Hooks in `%USERPROFILE%\.claude\settings.json` bleiben unangetastet – die GlassBar-Einträge dort bitte selbst entfernen.

## Bedienung

| Aktion | Wie |
|---|---|
| Ansicht wechseln (Apps / Spaces / Player / eigene) | Mausrad oder Touchpad-Wischen über der Dock-Mitte, oder Tabs anklicken |
| Panel öffnen | Aktiven Tab nochmal klicken, Cover klicken, Launcher-Button |
| Apps-Panel | Strg+Alt+D (im Panel: Strg+K = Suche, ↑/↓/Enter) |
| Player | Strg+Alt+P |
| Original-Taskleiste kurz zeigen | Strg+Alt+B |
| Space anwenden | Strg+Alt+1–9 |
| Lautstärke | Mausrad über den Statussymbolen oder dem Lautsprecher im Mini-Player |
| Schnellaktionen | Rechtsklick aufs Dock (Einstellungen, Bildschirmrand, Auto-Ausblenden, Taskleiste zeigen, Pause, Beenden) |
| Kalender | Klick auf die Uhr |
| App einer eigenen Ansicht zuordnen | Rechtsklick auf ein App-Symbol → „Ansichten“ |
| Claude: neue Sitzung mit Prompt (Claude-Modul an) | Strg+Alt+C |
| Claude-Freigabe per Tastatur (optional) | Strg+Enter erlauben, Strg+Rücktaste ablehnen (nur ausgewählte Anfrage, Claude-Panel aktiv) |

Wenn ein Hotkey schon von einer anderen App belegt ist, zeigt das Einstellungen → Bedienung → Tastenkürzel an.

## Einstellungen

Zahnrad im Schnellzugriff → „Einstellungen …“ oder Tray-Icon → „Einstellungen …“.
Hier lassen sich Aussehen, Position, Ansichten, Bedienung, Medien, Bildschirme und Systemverhalten einstellen.
Mit „Dock pausieren“ kommt vorübergehend die Windows-Taskleiste zurück.

## Aussehen & Position

- Darstellung: Dock, Notch (schmale Pille oben mittig, klappt bei Maus darauf mit Musik, Live-Songtext, Wetter, Termin, Timer, Claude und Apps auf) oder beides.
- Kompaktes Dock (Standard): rund 20 % kleiner, Tabs nur als Symbol. Die Dock-Höhe folgt der Symbolgröße, der reservierte Platz ebenso.
- Schrift: Manrope (eingebettet, SIL OFL) oder Segoe UI. Ein Schalter wechselt zwischen Dark und White Matte.
- Farbschema: Dunkel (Graphit), Hell oder „Wie Windows“ (folgt dem Windows-App-Modus live).
- Bildschirmrand: unten, oben, links oder rechts – auch je Bildschirm unterschiedlich (Bildschirme → „Eigene Position“). Seitlich zeigt das Dock die Apps senkrecht; Spaces und Player öffnen sich als Panel. „Links/Rechts“ bei der Ausrichtung bedeutet dort oben/unten.
- Feinabstimmung: Größe der Oberfläche (85–130 %), Leuchten (Aus/Dezent/Kräftig/Neon), Hoher Kontrast, Deckkraft, Symbolgröße. Änderungen sind sofort in der Vorschau und im Dock sichtbar; „Abbrechen“ stellt den alten Stand wieder her.
- Module (Uhr, System, Kalender, Timer, Wetter, Claude) lassen sich links, mittig oder rechts platzieren und umsortieren.
- Eigene Ansichten: zusätzliche Tabs mit eigener App-Gruppe (Ansichten & Module → Eigene Ansichten).
- Automatisch ausblenden, optional als Ruhe-Kapsel (kleine Pille mit Uhrzeit und laufendem Titel).
- In Vollbild ausblenden erkennt auch Spiele im randlosen Fenstermodus.

### Vorschau in den Einstellungen

Auf allen Seiten, die das Aussehen betreffen (Aussehen, Design-Studio, Position & Größe, Ansichten & Module, Widgets, Meldungen, Medien), steht rechts eine Live-Vorschau: ein Mini-Dock mit deinen Farben, Ecken, Rahmen, Schatten, Schrift, Akzent, Leuchten, Symbolgröße und -abstand, Position am Bildschirmrand und den eingeschalteten Ansichten und Modulen – dazu je Seite das Passende (Panel-Karte, Meldung, Player, Widget, Notch). Sie bleibt beim Scrollen stehen und zeigt jede Änderung sofort, noch bevor das echte Dock folgt. In schmalen Fenstern rutscht sie als kleinerer Streifen über die Einstellungen.

### Design-Studio

- 17 Vorlagen, getrennt nach Dunkel (Graphit, Mitternacht, Nord, Sakura, Terminal, Abendrot (früher „Sonnenuntergang“), Tiefsee, Moos, Espresso, Aurora, Kontrast) und Hell (Kreide, Papier, Lavendel, Gletscher, Rosé, Salbei). Eine Vorlage setzt alle Werte und die Akzentfarbe; danach lässt sich alles anpassen.
- Farben: Glas, Text, Flächen, Kanten und optional eine **zweite Akzentfarbe** – Regler, Schalter, Dock-Markierungen, Fortschrittsbalken, Visualizer und Leuchtkanten laufen dann als Verlauf.
- Form & Rahmen: Ecken (rund/klein/eckig), **Rahmen** (ohne/fein/kräftig) und Kanten-Stärke.
- Dock: **Symbolabstand** (2–20 px) und **Dock-Höhe** (Polsterung um die Symbole).
- Effekte: Schatten, **Akzent-Schimmer** (wie stark der Akzent im unteren Glas leuchtet) und Körnung.
- „Exportieren“ speichert alle Werte samt Akzent als JSON; ältere Design-Dateien ohne die neuen Werte laden weiter mit den bisherigen Standards.

## Live-Aktivitäten (Notch)

Was gerade läuft, zeigt die eingeklappte Notch links neben der Uhr: Symbol in einem dünnen Fortschrittsring plus kurzer Text, bei mehreren Aktivitäten ein Zähler („+2“). Aufgeklappt stehen alle Aktivitäten als kleine Kacheln mit Fortschrittsbalken über den Karten; ein Klick öffnet die passende Stelle (Download-Ordner bzw. fertige Datei im Explorer, Timer starten/pausieren, Claude-Panel, Pull Request). Fertiges bekommt kurz einen grünen Haken (mit kleinem Hüpfer, außer bei „Bewegung reduzieren“) und verschwindet nach rund 4 Sekunden.

- Downloads: `.crdownload`, `.part`, `.partial`, `.download`, `.opdownload` im Download-Ordner; `.tmp` erst, wenn die Datei wächst. Browser verraten die Gesamtgröße nicht, daher meist Größe und Tempo statt Prozent. Liest nur Namen und Größen, nichts wird gesendet.
- Timer: Fokus-Timer und Pomodoro (sofern eingeschaltet) mit Restzeit; pausierte Timer bleiben gedimmt sichtbar.
- Claude Code: „arbeitet …“ bzw. „wartet auf Freigabe“ je Sitzung, danach kurz „fertig“.
- GitHub-CI: laufende Checks eigener PRs aus den Daten des GitHub-Widgets (keine eigenen Abfragen; Stand also wie das Widget alle 5 Minuten).

Jede Quelle lässt sich unter Einstellungen → Position & Größe → Live-Aktivitäten abschalten. „Auch im Dock zeigen“ (Standard aus) legt einen kleinen Knopf mit der wichtigsten Aktivität ins Dock; er lässt sich wie andere Module platzieren. Ein Sekundentakt läuft nur, solange ein Download wächst oder ein Haken ausgeblendet wird.

## Widgets (Einstellungen → Widgets)

Alle standardmäßig aus; Klick aufs Widget im Dock öffnet seine Ansicht, Position unter „Ansichten & Module“.

- Lokal: Systemmonitor (CPU/GPU/RAM), Netzwerk-Rate, Zwischenablage (nur im Arbeitsspeicher, Passwort-Manager werden übersprungen), Pomodoro, Notizen/To-do (`%AppData%\GlassBar\notes.json`), Downloads (Ziehen in andere Apps), Papierkorb, Mikrofon & Kamera, Geräte-Akku (Bluetooth, Controller), Weltuhr.
- Mit Internet, erst nach dem Einschalten: GitHub (über die angemeldete GitHub CLI `gh`, kein Token in GlassBar), Dienste (eigene URLs oder `host:port`, Prüfung alle 5 Minuten), Kurse (CoinGecko, Stooq).
- Benachrichtigungen (lokal): liest die Windows-Benachrichtigungen (nur im Arbeitsspeicher), gruppiert nach App mit Uhrzeit; „Verwerfen“ entfernt eine, „Alle löschen“ leert das Infocenter, ein Klick holt die App nach vorn. Die Zahl im Dock leuchtet bei neuen Meldungen; Apps im Dock bekommen ein kleines Badge mit ihrer Anzahl – nur wenn die Zuordnung sicher ist (AUMID, Paket, Programmpfad oder eindeutiger Name). Beim ersten Einschalten fragt Windows nach dem Zugriff; verweigert öffnet „Windows-Einstellungen öffnen“ Datenschutz → Benachrichtigungen. Je nach Windows-Version gibt Windows die Benachrichtigungen nur an Store-Apps heraus – dann sagt das Widget das, statt etwas anzuzeigen. Aktualisiert alle 4 Sekunden, solange es eingeschaltet ist (nicht während eines Spiels im Vollbild).

### Bildschirmfotos

- „Bereich“ (oder Strg+Alt+S, änderbar unter Tastenkürzel, aktiv nur mit eingeschaltetem Widget): ein Standbild aller Bildschirme erscheint abgedunkelt, Rechteck aufziehen und loslassen übernimmt; Enter nimmt den ganzen Bildschirm unter dem Mauszeiger, Esc oder Rechtsklick bricht ab.
- „Bildschirm“: der Monitor unter dem Mauszeiger. „Fenster“: das oberste App-Fenster, in dem du zuletzt gearbeitet hast (ohne GlassBar).
- Gespeichert als PNG in `Bilder\Screenshots\GlassBar` (der Windows-Screenshot-Ordner, in den auch Win+Druck speichert – folgt einem verschobenen oder OneDrive-Bilderordner; der eigene Unterordner hält den Verlauf frei von fremden Aufnahmen) und zusätzlich in der Zwischenablage.
- Die letzten 8 Aufnahmen als Vorschau: Klick öffnet, Ziehen legt die Datei in andere Apps, „Text erkennen“ liest den Text mit der Windows-Texterkennung (lokal, ohne Internet) und kopiert ihn. „Bereich → Text“ nimmt auf und erkennt direkt, ohne zu speichern.
- Texterkennung braucht eine Windows-Sprache mit „Optische Zeichenerkennung“ (Einstellungen → Zeit und Sprache → Sprache); fehlt sie, zeigt das Widget einen Hinweis mit Knopf zu den Spracheinstellungen.
- Smart Home (Home Assistant): Adresse (`http://homeassistant.local:8123`), Long-Lived Access Token (Home Assistant → Profil → Sicherheit; GlassBar speichert es nur DPAPI-verschlüsselt für dein Windows-Konto) und Entitäten eintragen, eine pro Zeile – `light.kueche` oder `Küche | light.kueche`. „Verbindung testen“ fragt `/api/` ab. Lichter (an/aus, Helligkeit), Schalter/Ventilatoren/`input_boolean` (umschalten), Szenen und Skripte („Ausführen“), Rollos (auf/stopp/zu), Sensoren und Thermostate (Anzeige). Aktualisiert alle 30 s und beim Öffnen; das Dock zeigt die Zahl eingeschalteter Lichter. Nur http/https, Weiterleitungen werden nicht verfolgt.

### Schnellnotizen

- **Strg+Alt+N** (änderbar unter Einstellungen → Tastenkürzel) öffnet überall ein kleines Eingabefenster am Dock: tippen, **Enter** (oder Strg+Enter) speichert, Umschalt+Enter macht eine neue Zeile, **Esc** verwirft. Klickst du nur woanders hin, bleibt der Entwurf fürs nächste Öffnen erhalten. Auch per „Neue Notiz“ im Notizen-Widget.
- Mit eingeschaltetem Kalender-Modul lässt sich die Notiz mit einem Termin der nächsten Tage verknüpfen; ein Termin, der gerade läuft oder in weniger als 30 Minuten beginnt, ist schon ausgewählt.
- Das Notizen-Widget listet alle Notizen (neueste oben, mit Termin-Chip) – direkt bearbeiten, löschen, Verknüpfung ändern oder entfernen. Die To-do-Liste bleibt darunter.
- Kalender-Karte (Klick auf die Uhr): „Anstehend“ zeigt die nächsten Termine, ein Notiz-Symbol markiert Termine mit Notizen; ein Klick zeigt und bearbeitet sie oder legt eine neue an.
- Fünf Minuten vor einem Termin mit Notiz erscheint eine Meldung mit der Notiz (nur solange GlassBar läuft); die Notch zeigt beim nächsten Termin einen Notiz-Hinweis.
- Zuordnung: zuerst über die UID des Termins, sonst über Beginn und Titel – ein verschobener Termin oder eine neue UID nach dem Export behält seine Notizen. Wiederkehrende Termine werden weiterhin nicht erweitert.
- Alles bleibt lokal in `notes.json`; die frühere einzelne Notiz wird beim ersten Start als erste Notiz übernommen.

## Design prüfen (Entwickler)

```
GlassBar.exe --render-shell <Ordner> [light]
```

Nur im Debug-Build: rendert Dock und Notch (eingeklappt, Meldung, aufgeklappt) als PNG, ohne die echten Fenster zu zeigen. Ergänzt `--render-settings` (alle Seiten in der Standardgröße 1340 × 900, dazu das Design-Studio im schmalen Fenster) und `--render-panels`.

## Tests

```
dotnet run --project tests/GlassBar.Tests
```

Prüft Rechner, Suche, Modul-Anordnung, Widget-Einstellungen und baut das Einstellungsfenster unsichtbar auf (nur lesend). Exit-Code = Anzahl Fehler.

## Updates

Einstellungen, Pins, Spaces und Notizen liegen in `%AppData%\GlassBar` – getrennt vom Programmordner, ein Update ersetzt nur das Programm. Vor dem ersten Start einer neuen Version legt GlassBar eine Sicherung in `%AppData%\GlassBar\backups` an.


**Über GitHub:** Jede neue Version auf `main` wird automatisch zu einem Release mit fertigen Paketen. Mit Einstellungen → System → „Updates von GitHub“ prüft GlassBar zusätzlich dort (über die angemeldete GitHub CLI `gh`, kein Token in GlassBar) und installiert per „Jetzt aktualisieren“ das passende Paket – auch auf PCs ohne Quellcode.

`scripts\Install.ps1` merkt sich den Projektordner. Die installierte Version prüft lokal, ob dort eine neuere Version liegt, und meldet sich; „Jetzt aktualisieren“ unter Einstellungen → System führt das Skript aus und startet GlassBar neu. Nichts geht ins Netz.

## Profile

Profile speichern Aussehen, Position, Ansichten, Bedienung und Medien (Bereiche einzeln wählbar), lassen sich duplizieren, exportieren und importieren. Optional schalten sie automatisch um – im Akku, am Netzteil, bei einem oder mehreren Bildschirmen – aber nur beim Zustandswechsel.

## Umrechnen in der Suche

Im Apps-Panel (Strg+Alt+D, Strg+K) rechnet die Suche neben Rechenaufgaben auch Einheiten um: Zahl, Einheit, „in“/„to“/„nach“/„=“, Zieleinheit – etwa „5 km in mi“, „12,5 kg to lb“, „100 °F in °C“, „3 h in min“, „1,5 GB in MB“ (KB = 1000, KiB = 1024 Byte), „20 m/s in km/h“, „2 l in ml“. Länge, Masse, Temperatur, Volumen, Fläche, Geschwindigkeit, Zeit und Datenmengen, deutsche und englische Namen, Komma oder Punkt als Dezimaltrenner. Das läuft immer offline; Enter kopiert die Zahl.

Währungen („100 eur in usd“, „50 $ in €“) erst nach Einstellungen → Bedienung → „Währungen umrechnen“ (Standard: aus). Dann lädt GlassBar höchstens alle 12 Stunden die Referenzkurse der EZB (ohne Konto) nach `%AppData%\GlassBar\currency-rates.xml`; ohne Netz gilt der letzte Stand, das Kursdatum steht unter dem Ergebnis.

## Songtexte (LRCLIB, optional)

Standardmäßig aus. Aktivieren direkt im Player (Knopf „Aa“ → „Songtexte aktivieren“) oder unter Einstellungen → Medien. Erst danach gehen Titel, Interpret und Länge des laufenden Songs an `lrclib.net`; ohne Interpret wird nichts gesucht. Die aktuelle Zeile erscheint dann auch im Mini-Player des Docks und in der Notch. Ergebnisse liegen in `%AppData%\GlassBar\lyrics` und lassen sich per „Leeren“ entfernen.

## Claude Code (optional)

1. Einstellungen → Claude Code → Modul einschalten und übernehmen.
2. Die angezeigte Konfiguration kopieren und selbst in `%USERPROFILE%\.claude\settings.json` unter `hooks` einfügen – bestehende Hooks dabei nicht überschreiben, sondern ergänzen.
3. Optional „Freigaben im Dock beantworten“ aktivieren. Bei Zeitablauf, Verbindungsfehler oder wenn GlassBar nicht läuft, wird nie automatisch erlaubt – dann fragt das Terminal wie gewohnt.

Nach einer Installation per Skript die Konfiguration neu kopieren, da sich der Pfad zu `GlassBar.Hook.exe` ändert. WSL wird nicht unterstützt.

**Tokens & Kosten:** Die Claude-Karte liest die lokalen Transkripte unter `%USERPROFILE%\.claude\projects` (nur solange die Karte offen ist, nur neu angehängte Zeilen) und zeigt pro laufender Sitzung Tokens und ca. Kosten, darunter „Letzte Sitzungen“ (Klick öffnet den Projektordner) und die Summe von heute. Die Kosten sind eine Schätzung nach den öffentlichen API-Listenpreisen ohne Gewähr – mit einem Claude-Abo zahlst du nicht pro Token. Bei unbekanntem Modell steht „–“.

**Schnell-Prompt:** Strg+Alt+C (änderbar unter Bedienung → Tastenkürzel) oder „Neue Sitzung“ in der Claude-Karte öffnet ein kleines Eingabefenster: Projektordner wählen (zuletzt genutzte Projekte oder „Ordner wählen …“), Prompt eingeben, Enter startet `claude` in einem neuen Windows-Terminal-Tab (ohne Windows Terminal in einem Konsolenfenster). Der Prompt wird nie über eine Befehlszeile oder Shell weitergereicht, sondern über eine private Temp-Datei an `GlassBar.Hook.exe`, das `claude` direkt startet. Nach dem Beenden bleibt eine PowerShell im Ordner offen. Benötigt `claude.exe` (nativer Installer) oder eine npm-Installation mit Node.

## Spotify (Warteschlange + Favorit)

1. Unter https://developer.spotify.com/dashboard eine App anlegen.
2. Als Redirect-URI `http://127.0.0.1:47812/callback` eintragen.
3. Die Client-ID im Media-Panel einfügen und auf „Verbinden“ klicken.

Die Warteschlange kann über die API nur gelesen werden; „Jetzt abspielen“ braucht Spotify Premium.

## Infobereich

Der Pfeil im Dock zeigt die Symbole anderer Programme (Discord, Steam …). Klicks gehen wie bei der Windows-Taskleiste an das Programm, Mittelklick heftet ein Symbol direkt ins Dock. Technisch meldet sich GlassBar als zusätzliches Taskleistenfenster und reicht jede Meldung an die Windows-Taskleiste weiter – „Original-Taskleiste zeigen“ hat also weiterhin alle Symbole. Abschalten: Einstellungen → Ansichten & Module → Infobereich.

## Bluetooth-Geräte

Die Schnelleinstellungen listen gekoppelte Geräte. Kopfhörer/Headsets verbinden und trennen dort per Knopf – GlassBar nutzt dafür denselben Schalter des Bluetooth-Audiotreibers wie die Windows-Einstellungen. Andere Geräte (Controller, Maus) zeigen Status und Akku. Nur lesend prüfen: `dotnet run --project tests/GlassBar.Tests -- --probe-bluetooth`.

## Windows-Taste (optional)

Einstellungen → Bedienung → „Windows-Taste öffnet das Apps-Panel“. Ein Tastatur-Hook auf eigenem Thread erkennt die allein gedrückte Win-Taste; beim Loslassen schickt GlassBar einen unbelegten Tastencode (0xE8) vor dem Loslassen, damit Windows das Startmenü nicht öffnet (gleiches Verfahren wie PowerToys). Kürzel mit Win bleiben unverändert, Strg+Esc öffnet das Startmenü.

## Audio-Visualizer

Einstellungen → Medien → „Audio-Visualizer“. GlassBar hört per WASAPI-Loopback lokal mit, was die Lautsprecher abspielen, und berechnet 24 Frequenzbänder (nichts wird gespeichert oder gesendet). Läuft nur während der Wiedergabe, pausiert im Vollbild und bei „Bewegung reduzieren“. Prüfen ohne GlassBar: `dotnet run --project tests/GlassBar.Tests -- --probe-visualizer`.

## Absturzschutz

Mit GlassBar startet ein kleiner Wächterprozess (`GlassBar.exe --watchdog`, ohne Fenster). Endet GlassBar unerwartet, holt er die Windows-Taskleiste zurück, schreibt `crash.log` und startet GlassBar neu – höchstens 3-mal in 10 Minuten. Normales Beenden, Task-Manager und `Stop-Process` lösen keinen Neustart aus.

## Daten

- Fehlerprotokoll `error.log` nur, wenn Einstellungen → System → „Lokale Diagnose“ an ist (Standard: aus).
- Einstellungen: `%AppData%\GlassBar\settings.json` (Pins, Spaces, Aussehen). Das Spotify-Token ist per DPAPI verschlüsselt.
- Space-Vorschauen: `%AppData%\GlassBar\spaces\*.png`
- Caches: `weather-cache.json`, `lyrics\`, `currency-rates.xml` (nur wenn die Module aktiv sind)
- Absturzprotokoll `crash.log` (immer, klein, nur lokal) und `crash-history.txt` für die Neustart-Grenze
- Angeheftete Zwischenablage-Einträge: `clipboard-pins.json` und Bilder in `clipboard-pins\`

## Wenn die Taskleiste nach einem Absturz fehlt

`GlassBar.exe --restore-taskbar` ausführen (bei Installation per Skript: Startmenü → „GlassBar - Taskleiste wiederherstellen“). Das funktioniert auch, wenn GlassBar abgestürzt ist. Alternativ den Explorer im Task-Manager neu starten.

## Wenn das Dock leer erscheint (nur Glasfläche, keine Symbole)

Dann hängt die Grafikbeschleunigung von Windows (betrifft alle WPF-Programme, nicht nur GlassBar). Grafiktreiber mit Win+Strg+Umschalt+B neu starten oder den PC neu starten. Hilft das nicht dauerhaft: Einstellungen → System → „Software-Rendering“ einschalten und GlassBar neu starten.

## Sprache / Language

Deutsch und Englisch. Einstellungen → System → Sprache: „Wie Windows“, „Deutsch“ oder „English“ (gilt nach Neustart).
Neue Oberflächentexte: deutsch schreiben, in C# mit `Loc.T("…")`/`Loc.F("… {0}", wert)`, in XAML mit `{loc:Tr '…'}`, und die englische Übersetzung in `src/GlassBar/Localization/en.json` eintragen – ein Test prüft, dass keine fehlt.
