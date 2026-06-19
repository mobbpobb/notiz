# Vorbereitung
_________________________________
## 1. Werkzeuge

### 1-1. Node.js (LTS) installieren
Website öffnen: https://nodejs.org
```
> Get Node.js
> Option: *LTS* und Windows Installer (`.msi`)
> Während der Installation immer *Weiter* klicken.
```
In Windows-`CMD` prüfen:
```cmd
node -v
npm -v
```

### 1-2. Docsify-CLI installieren
```cmd
npm i docsify-cli -g
```
_________________________________
## 2. Docsify-Projekt anlegen
```cmd
cd C:\work
mkdir docs
cd docs
docsify init .
```
Danach gibt es `index.html` und `README.md` im Ordner **docs**.
_________________________________
## 3. Eigene Struktur
```md
docs
 ├─README.md (Übersicht / Startseite)  
 ├─_sidebar.md (Navigation)  
 ├─lab-umgebung
   ├─aktuelle-einstellungen.md
 ├─windows-server-2022
   ├─storage-spaces
    ├─overview.md  
    ├─windows-server-2022/storage-spaces/raid-0-simple-layout.md
    ├─windows-server-2022/storage-spaces/raid-1-mirror.md  
    ├─windows-server-2022/storage-spaces/raid-5-parity.md
```

in `_sidebar.md`
```md
- [Startseite](/)

- Lab-Umgebung
  - [Aktuelle Einstellungen](#/lab-umgebung/aktuelle-einstellungen)

- Datenträger / Storage Spaces
  - [Überblick](#/windows-server-2022/storage-spaces/overview)
  - [RAID 0 ähnlich: Simple](#/windows-server-2022/storage-spaces/raid-0-simple)
  - [RAID 1 ähnlich: Mirror](#/windows-server-2022/storage-spaces/raid-1-mirror)
  - [RAID 5 ähnlich: Parity](#/windows-server-2022/storage-spaces/raid-5-parity)
```
_________________________________
## 4. Docsify konfigurieren
In `index.html`:
```html
<script>
  window.$docsify = {
    name: 'mein Notiz',
    repo: '',
    loadSidebar: true,
    subMaxLevel: 0,
    alias: {
      '/.*/_sidebar.md': '/_sidebar.md'
    }
  }
</script>
```
_________________________________
## 5. Lokalen Server starten

```cmd
cd C:\work\docs
docsify serve .
```
Dann im Browser öffnen:
```
http://localhost:3000
```