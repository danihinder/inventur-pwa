# Wartung & Betrieb – Inventur Scanner

Persönliche Betriebsanleitung. Nicht für andere bestimmt.

---

## App-Zugang

- **URL:** `https://danihinder.github.io/inventur-pwa/`
- **Repo:** `https://github.com/danihinder/inventur-pwa` (privat)
- Lokale Dateien: `C:\sync\moebius\inventur-pwa\`

---

## Masterlist aktualisieren (bei neuer Stammdaten-XLSX)

### Voraussetzungen (einmalig)
```
pip install openpyxl
```

### Normaler Ablauf

Die XLSX-Quelldatei kommt aus PART_MGMT und liegt standardmässig hier:
```
C:\sync\moebius\PART_MGMT\input\Inventory value by item number, planned status, and age.xlsx
```

Wenn die Datei dort liegt, reicht:
```bash
cd C:\sync\moebius\inventur-pwa
python generate_masterlist.py
```

Das Script:
1. Liest die XLSX ein und konvertiert sie nach `data/masterlist.json`
2. Macht automatisch `git add`, `git commit` und `git push`
3. GitHub Pages aktualisiert sich innerhalb ~1 Minute

### Andere Quelldatei angeben
```bash
python generate_masterlist.py --input "C:\pfad\zur\datei.xlsx"
```

### Nur JSON erzeugen, kein Git-Push
```bash
python generate_masterlist.py --no-push
```

### Was das Script validiert
Beim Start prüft es ob die erwarteten Spalten (SUB INVENTORY, ITEM NUMBER, MIN QTY) an den
richtigen Positionen stehen. Bei Abweichung erscheint eine Warnung mit dem gefundenen Spaltennamen.
In dem Fall müssen die Spaltenindizes in `generate_masterlist.py` (Zeilen ~39–44) angepasst werden.

---

## Manueller Git-Push (falls automatisch fehlgeschlagen)

```bash
cd C:\sync\moebius\inventur-pwa
git add data/masterlist.json
git commit -m "Masterlist Update"
git push
```

GitHub braucht danach ca. 1 Minute bis die neue Version live ist.

---

## GitHub Pages einrichten (einmalig – bereits erledigt)

Nur zur Dokumentation, falls das Repo neu aufgesetzt werden muss:

1. Repo erstellen: https://github.com/new
   - Name: `inventur-pwa`
   - Visibility: **Private**
2. Dateien hochladen:
   ```bash
   git init
   git add .
   git commit -m "Inventur PWA initial"
   git branch -M main
   git remote add origin https://github.com/danihinder/inventur-pwa.git
   git push -u origin main
   ```
3. GitHub Pages aktivieren:
   - Repo → Settings → Pages
   - Source: **Deploy from a branch**, Branch: `main` / `/ (root)`
   - Save

---

## Expert Mode

Zeigt in der Abschluss-Auswertung den Sollbestand (QOH) und die noch fehlende Stückzahl –
versteckt vor normalen Benutzern damit niemand „bescheisst".

**Aktivieren:** 7× schnell auf „Build XX" im App-Header tippen → PIN eingeben: `675756`
**Deaktivieren:** nochmals 7× auf „Build XX" tippen

Wenn aktiv: „Build XX" leuchtet orange.
Der Zustand bleibt nach App-Neustart gespeichert.

---

## Build-Nummer hochzählen

Bei jeder Änderung an `index.html` oder `sw.js` wird die Build-Nummer erhöht.
Das zwingt den Browser, den neuen Service Worker zu laden und die App zu aktualisieren.

In `index.html` suchen nach `Build XX` → Zahl erhöhen.
In `sw.js` suchen nach `inventur-vXX` → Zahl erhöhen.

---

## Typische Probleme

| Problem | Lösung |
|---|---|
| App zeigt alte Version | Hard Reload: iOS Safari → Tab schliessen + neu öffnen. Android Chrome → ⋮ → Neu laden |
| QR-Code-Scan funktioniert nicht | Kamera-Berechtigung in den Browser-Einstellungen prüfen |
| Masterlist-Script findet Datei nicht | Pfad mit `--input` explizit angeben |
| Git push schlägt fehl | `git status` prüfen, ggf. `git pull` davor |
