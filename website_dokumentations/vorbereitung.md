# Vorbereitung
_________________________________
## Werkzeuge

### 1. Node.js (LTS) installieren
Website öffnen: https://nodejs.org
```
> Grünen **LTS**-Button klicken.
> Die `.msi`-Datei starten.
> Immer **Weiter** klicken.
> Standard-Einstellungen lassen.
```
In Windows-`CMD` prüfen:
```cmd
node -v
npm -v
```

### 2-1. Docsify-CLI installieren
```cmd
npm i docsify-cli -g
```
### 2-2. Docsify-Projekt anlegen
```cmd
cd C:\work
mkdir docs
cd docs
docsify init .
```
Danach gibt es `index.html` und `README.md` im Ordner **docs**.

### 3. Eigene Struktur
Im Ordner **docs** liegen zum Beispiel diese Dateien:
- `README.md` – Übersicht / Startseite
- `_sidebar.md` – Navigation
- `lab-umgebung/aktuelle-einstellungen.md`
- `windows-server-2022/storage-spaces/overview.md`
- `windows-server-2022/storage-spaces/raid-0-simple-layout.md`
- `windows-server-2022/storage-spaces/raid-1-mirror.md`
- `windows-server-2022/storage-spaces/raid-5-parity.md`

####  Beispiel für `_sidebar.md`

```md
- [Startseite](/)

- Lab-Umgebung
  - [Aktuelle Einstellungen](#/lab-umgebung/aktuelle-einstellungen)

- Datenträger / Storage Spaces
  - [Überblick](#/windows-server-2022/storage-spaces/overview)
  - [RAID 0 ähnlich: Simple Layout](#/windows-server-2022/storage-spaces/raid-0-simple-layout)
  - [RAID 1 ähnlich: Mirror](#/windows-server-2022/storage-spaces/raid-1-mirror)
  - [RAID 5 ähnlich: Parity](#/windows-server-2022/storage-spaces/raid-5-parity)
```

### Beispiel für `lab-umgebung/aktuelle-einstellungen.md`

```md
## Aktuelle Einstellungen

| Plattform | OS | Virtual Switch | IP-Adresse | Internet | Status |
|---|---|---|---|---|---|
| Hyper-V | Server 2022 | intern & extern | 192.168.10.10 / 10.60.47.xxx | ja | Lokal |
| Hyper-V | Windows 11 | intern | 192.168.10.106 | ja (NAT) | Domäne / Lokal |
| Hyper-V | Ubuntu | intern & extern | 192.168.10.105 / 10.60.47.xxx | ja | Lokal |
| Tower (Host) | Windows 11 | intern & default | 192.168.10.10x / 10.60.47.xxx | ja | Lokal |
```

## Docsify konfigurieren
In `index.html`:

```html
<script>
  window.$docsify = {
    name: 'Windows Server 2022 – Storage Spaces',
    repo: '',
    loadSidebar: true,
    subMaxLevel: 0,
    alias: {
      '/.*/_sidebar.md': '/_sidebar.md'
    }
  }
</script>
```

## Lokalen Server starten

```bash
cd C:\work\docs
docsify serve .
```

Dann im Browser öffnen:

```text
http://localhost:3000
```

## Sidebar-Hauptpunkte

### Problem
Wenn ein Hauptpunkt wie **Lab-Umgebung** klickbar aussieht, aber keine sichtbare Änderung bringt, ist das leicht missverständlich.

### Gute Lösung
Hauptpunkte nur als **Text** anzeigen.  
Nur die Unterpunkte sind echte Links.

```md
- [Startseite](/)

- Lab-Umgebung
  - [Aktuelle Einstellungen](#/lab-umgebung/aktuelle-einstellungen)

- Datenträger / Storage Spaces
  - [Überblick](#/windows-server-2022/storage-spaces/overview)
  - [RAID 0 ähnlich: Simple Layout](#/windows-server-2022/storage-spaces/raid-0-simple-layout)
  - [RAID 1 ähnlich: Mirror](#/windows-server-2022/storage-spaces/raid-1-mirror)
  - [RAID 5 ähnlich: Parity](#/windows-server-2022/storage-spaces/raid-5-parity)
```

Alternative:  
Hauptpunkte bekommen eine eigene Übersichtsseite.  
Das ist gut, wenn später mehr Inhalte dazukommen.