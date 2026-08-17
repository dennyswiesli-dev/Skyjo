# Skyjo Wertungsblock

Punktezähler für das Kartenspiel [Skyjo](https://kleopas.de/skyjo/) (Magilano). Läuft als installierbare Web-App komplett im Browser — kein Server, kein Konto, keine Daten verlassen das Gerät.

## Funktionen

- **Spieler verwalten** — 2 bis 8 Personen, jederzeit umbenennbar
- **Verdopplungsregel automatisch** — wer die Runde beendet, muss allein am tiefsten liegen; sonst zählt seine positive Rundenpunktzahl doppelt. Die App prüft das beim Tippen und sagt vorab, was gewertet wird.
- **Rangliste** mit Fortschritt Richtung Zielpunktzahl (50 / 100 / 150 / 200 wählbar)
- **Wertungsblock** — alle Runden als Tabelle, mit Markierung wer beendet hat und wo verdoppelt wurde
- **Statistik pro Spieler** — Durchschnitt, beste und schlechteste Runde, Rundensiege, wie oft beendet, wie oft verdoppelt, Minusrunden
- **Offline nutzbar** über Service Worker, Punktestand bleibt nach dem Schliessen erhalten
- Einzelne Runden nachträglich löschbar, falls sich jemand vertippt hat

## Wertungsregeln im Detail

Der Spieler, der alle Karten aufgedeckt und damit die Runde beendet hat, bekommt seine Rundenpunktzahl verdoppelt, wenn:

1. er **nicht allein die tiefste** Punktzahl hat (Gleichstand zählt als verloren), **und**
2. sein Rundenergebnis **positiv** ist

Verdoppelt wird das Gesamtergebnis inklusive Minuskarten, also `(12 − 4) × 2 = 16` und nicht `(12 × 2) − 4`. Die Partie endet, sobald jemand die Zielpunktzahl erreicht; gewonnen hat, wer insgesamt am wenigsten Punkte hat.

## Dateien

```
index.html               komplette App (HTML, CSS, JS in einer Datei)
manifest.webmanifest     Web App Manifest für die Installation
sw.js                    Service Worker für Offline-Betrieb
icon.svg                 Vektor-Logo
favicon.ico              Favicon für ältere Browser
icons/                   PNG-Icons in allen benötigten Grössen
  ├─ icon-192.png / icon-512.png                 Standard
  ├─ icon-maskable-192.png / -512.png            Android adaptive icons
  ├─ apple-touch-icon.png                        iOS Homescreen (180 px)
  └─ favicon-16.png / favicon-32.png             Browser-Tab
.nojekyll                verhindert Jekyll-Verarbeitung auf GitHub Pages
```

## Auf GitHub Pages veröffentlichen

1. Neues Repository anlegen, alle Dateien unter Beibehaltung der Ordnerstruktur ins Root-Verzeichnis hochladen
2. **Settings → Pages → Source:** Branch `main`, Ordner `/ (root)`, speichern
3. Nach ein bis zwei Minuten ist die App unter `https://<benutzername>.github.io/<repo>/` erreichbar

Ein Service Worker braucht HTTPS — GitHub Pages liefert das automatisch. Lokal per Doppelklick geöffnet (`file://`) funktioniert die App ebenfalls, nur ohne Offline-Cache und ohne Installation.

## Als App installieren

**iPhone / iPad (Safari)** — Seite in Safari öffnen, Teilen-Symbol antippen, *Zum Home-Bildschirm*, bestätigen. Die App startet danach im Vollbild ohne Safari-Leiste. Wichtig: iOS erlaubt das nur in Safari, nicht in Chrome oder Firefox.

**Android (Chrome)** — Der Knopf *App installieren* erscheint automatisch unten auf der Seite, alternativ über das Menü *Zum Startbildschirm hinzufügen*.

**Desktop (Chrome / Edge)** — Installationssymbol rechts in der Adressleiste.

## Nach Änderungen

Der Service Worker liefert die zwischengespeicherte Version aus. Nach inhaltlichen Änderungen die Versionsnummer in `sw.js` hochzählen:

```js
const CACHE = "skyjo-v2";
```

Sonst sehen bereits installierte Geräte weiterhin den alten Stand.

## Datenschutz

Alle Punktestände liegen im `localStorage` des Geräts. Es gibt keinen Server, keine Analyse und keine Übertragung. Blockiert der Browser den Speicher, läuft die App trotzdem — der Stand geht dann beim Schliessen verloren.

Einzige externe Ressource sind die Schriften von Google Fonts. Wer das vermeiden möchte, entfernt die beiden `<link>`-Zeilen zu `fonts.googleapis.com` im `<head>` der `index.html`; die App fällt dann auf Systemschriften zurück.
