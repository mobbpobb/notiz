# Speicherpool & RAID 0 (Simple)  

Simple verteilt Daten über mehrere Datenträger.
Es ist schnell, aber es gibt **keine Redundanz**.

## 1. Für „RAID (Simple)“ - Physische Datenträger hinzufügen x3
```
Hyper-V 
> Server-Einstellung 
> SCSI 
> Festplatte:Hinzufügen 
> Neu 
-> Disk0x, Fest, 10GB
```

## 2. Speicherpool erstellen 
```
Server-Manager
> Datei/Speicherdienste 
> Speicherpools: Aktualisieren 
> "Verfügbare Datenträger" auswählen 
> Aufgaben:Neu 
-> "SimplePool" 
-> Alle drei Datenträger auswählen 
```

## 3. Paritätsvolume erstellen
```
Server-Manager 
> aktualisieren 
> "SimplePool" auswählen 
> Virtuelle Datenträger: Aufgaben: Neu 
> "SimplePool" auswählen 
> Name:"SimpleDisk" 
> (ohne) Gehäuseinformationen aktivieren 
> "Simple" 
> Fest 
> Maximale 
> 
Neuer Assistent erscheint
> Vorbemerkungen 
> weiter und erstellen (F:)
```

## 4. Konfiguration prüfen
```
Server-Manager 
> aktualisieren 
> „SimplePool" auswählen und dann die Laufwerksnamen erscheinen
```

## 5. Prüfung ohne Redundanz
```
Server-Explorer
> Laufwerk „F:“ 
> Datei hinzufügen
```
```
Hyper-V 
> Server 
> eine Festplatte entfernen
```
```
Server-Manger 
> aktualisieren 
> Das Laufwerk wird mit einem Warnsymbol (!) angezeigt
```

-> Laufwerk „F:“ Zugriff auf die Dateien ist weiterhin möglich
-> aber nach einem Server-Neustart 
-> Kein Zugriff mehr auf Laufwerk „F:“ möglich

--> Anders als bei Parity ist eine Reparatur nicht möglich, da die 3 Festplatten als ein einziges Simple-Volume (Striped/Spanning) erstellt wurden
