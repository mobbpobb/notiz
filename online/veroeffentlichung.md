# Veröffentlichung

## Manuelle Veröffentlichung mit Cloudflare

### Erstellung
1. Cloudflare öffnen.
2. **Workers und Pages** öffnen.
3. **Anwendung erstellen** klicken.
4. **Upload your static files** wählen.
5. Den Ordner **docs** hochladen.
6. Einen Projektnamen eingeben, zum Beispiel `notiz`.
7. **Bereitstellen** klicken.
8. Danach **Besuchen Sie** öffnen.

Wichtig:
- Hochgeladen wird der Ordner, in dem **`index.html` direkt liegt**.
- Bei diesem Weg werden Dateien **von Hand** hochgeladen.

### Aktualisierung
1. Das bestehende Projekt in Cloudflare öffnen.
2. **Neue Bereitstellung** wählen.
3. Den Ordner **docs** wieder hochladen.
4. **Bereitstellen** klicken.
5. Danach **Besuchen Sie** öffnen.

## Veröffentlichung mit GitHub

### GitHub vorbereiten
1. Auf GitHub ein **neues leeres Repository** anlegen.
2. Beispielname: `notiz`, `ws-docs` oder `docs-site`.
3. **Kein README** automatisch anlegen.
4. **Keine `.gitignore`** automatisch anlegen.
5. **Keine License** automatisch anlegen.

Wichtig:
- Das lokale `README.md` bleibt erhalten.
- Gemeint ist nur: GitHub soll **kein extra README** erzeugen.

### Lokale Git-Befehle
Das Terminal in `C:\work\docs` öffnen.

```bash
cd C:\work\docs
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/DEIN-NAME/DEIN-REPO.git
git push -u origin main
```

Wenn schon ein falsches `origin` gesetzt ist:

```bash
git remote remove origin
```

Danach neu setzen:

```bash
git remote add origin https://github.com/DEIN-NAME/DEIN-REPO.git
```

### Cloudflare mit GitHub verbinden
1. Cloudflare öffnen.
2. **Workers & Pages** öffnen.
3. **Create Application** klicken.
4. **Connect GitHub** wählen.
5. Das passende Repository auswählen.
6. Falls nötig, Mail bestätigen.

### Einstellungen in Cloudflare
Für diese einfache Docsify-Seite:

- **Framework preset:** None  
- **Build command:** leer lassen  
- **Root directory / Output directory:** nichts Besonderes nötig, wenn `docs` selbst das Projekt ist

### Aktualisierung mit GitHub

```bash
cd C:\work\docs
git add .
git commit -m "Änderung"
git push
```

Dann veröffentlicht Cloudflare die neue Version automatisch.

## Hinweis
Wenn schon ein manuelles Cloudflare-Projekt existiert und später noch ein GitHub-Projekt dazukommt, gibt es zwei Projekte.  
Dann am besten nur noch das **GitHub-Projekt** weiter benutzen.