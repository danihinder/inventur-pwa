# Inventur Scanner PWA

Barcode-Scanner für Lagerinventuren – läuft im Handy-Browser, kein App-Store nötig.

## GitHub Pages einrichten (einmalig, ~5 Minuten)

1. **Repo erstellen:** https://github.com/new
   - Repository name: `inventur-pwa`
   - Visibility: **Private** (empfohlen, weil Lager-Daten)
   - "Add a README file": nein

2. **Dateien hochladen:**
   ```
   git init
   git add .
   git commit -m "Inventur PWA initial"
   git branch -M main
   git remote add origin https://github.com/danihinder/inventur-pwa.git
   git push -u origin main
   ```

3. **GitHub Pages aktivieren:**
   - Repo → Settings → Pages
   - Source: **Deploy from a branch**
   - Branch: `main` / `/ (root)`
   - Save

4. **URL:** `https://danihinder.github.io/inventur-pwa/`
   → diese URL auf dem Handy öffnen ✅

## Wöchentliche Aktualisierung (neue Masterlist)

```bash
python generate_masterlist.py --input "pfad/zur/neuen/Inventory value...xlsx"
git add data/masterlist.json
git commit -m "Masterlist Update KW XX"
git push
```
GitHub Pages aktualisiert sich automatisch (~1 Minute).

## Auf dem Handy installieren

**Android Chrome:**
Adresszeile → ⋮ Menü → "Zum Startbildschirm hinzufügen"

**iOS Safari:**
Teilen-Symbol → "Zum Home-Bildschirm"

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | Die komplette App (kein Build-Schritt nötig) |
| `manifest.json` | PWA-Metadaten (Name, Icon, Display-Mode) |
| `data/masterlist.json` | Alle Teile + Min/QOH je Lager |
| `generate_masterlist.py` | Erzeugt masterlist.json aus XLSX |
