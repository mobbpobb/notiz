# Veröffentlichung

## Arbeitsordner

c:\work\docs

## Manuelle Veröffentlichung mit Cloudflare

### 1. Erstellung

Cloudflare:
> Compute  
> Workers und Pages  
> Anwendung erstellen  
> Upload your static files
> *Ordner* hochladen 
> Worker-Name eingeben
> Bereitstellen 
> Besuchen Sie

### 2. Aktualisierung

Cloudflare:
- *Worker-Name*
- Neue Bereitstellung
- *Ordner* hochladen
- Bereitstelle
- Besuchen Sie

_________________________________
## Veröffentlichung mit GitHub

### 1. Erstellung

#### 1-1. GitHub vorbereiten
GitHub:
> Create New 
> neues Repository

#### 1-2. Öffentlichen Ordner auf GitHub pushen
Windows CMD:
```
cd C:\work\docs
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/DEIN-NAME/DEIN-REPO.git
git push -u origin main
```
* Wenn schon ein falsches `origin` gesetzt ist:
```
git remote remove origin
git remote add origin https://github.com/DEIN-NAME/DEIN-REPO.git
```

#### 1-3. Cloudflare mit GitHub verbinden
Cloudflare:
> Workers und Pages
> Anwendung erstellen 
> Connect GitHub. Dann dein Repository auswählen (Optionen:none und leer)



### 2. Aktualisierung mit GitHub
Windows CMD:
```
cd C:\work\docs
git add .
git commit -m "Änderung"
git push
```