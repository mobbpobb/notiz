# Veröffentlichung

## Manuelle Veröffentlichung mit Cloudflare

### Erstellung
```
Cloudflare > "Workers und Pages" > "Anwendung erstellen" > "Upload your static files" > "docs"-Ordner hochladen > "Worker-Name": notiz > Bereitstellen -> "Besuchen Sie"
* "docs"-Ordner (in dem "index.html" direkt liegt) 
```

### Aktualisierung
```
"Worker-Name": notiz > "Neue Bereitstellung" > "docs"-Ordner hochladen > Bereitstellen -> "Besuchen Sie"
```

## Veröffentlichung mit GitHub

### Erstellung

#### GitHub vorbereiten
```
GitHub > Create New > "neues Repository"
```

#### Öffentlichen Ordner auf GitHub pushen
```cmd
cd C:\work\docs
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/DEIN-NAME/DEIN-REPO.git
git push -u origin main
```

* Wenn schon ein falsches `origin` gesetzt ist:
```cmd
git remote remove origin
git remote add origin https://github.com/DEIN-NAME/DEIN-REPO.git
```

#### Cloudflare mit GitHub verbinden
```
Cloudflare > "Workers und Pages" > "Anwendung erstellen" > Connect GitHub. Dann dein Repository auswählen (Optionen:none und leer)
```


### Aktualisierung mit GitHub
```cmd
cd C:\work\docs
git add .
git commit -m "Änderung"
git push
```