# Speicherpool & RAID 0 (Simple)  

Simple verteilt Daten über mehrere Datenträger.
Es ist schnell, aber es gibt `keine Redundanz`.

## Geplante Einstellungen
SimplePool (Disk04-06) - SimpleDisk - Simple (RAID0) 
MirrorPool (Disk07-09) - MirrorDisk - Mirror (RAID1) (2 Mirror und 1 Host-Spare)
ParityPool (Disk01-03) – ParityDisk - Parity (RAID5) 

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

## 5. Redundanz prüfen (Ergebnis: keine Redundanz)
```
Server-Explorer
> Laufwerk "F:" 
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
-> Anfangs ist der Zugriff auf die Dateien auf Laufwerk "F:" weiterhin möglich.
Nach einem Serverneustart ist jedoch kein Zugriff mehr auf Laufwerk "F:" möglich, sodass das System `ohne Redundanz` betrieben wird.

-> Anders als bei Parity ist eine `Reparatur nicht möglich`, da die drei Festplatten als ein einziges Simple-Volume (Striped/Spanning) erstellt wurden
