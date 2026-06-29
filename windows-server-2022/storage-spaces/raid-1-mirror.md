# Speicherpool & RAID 1 (Mirror)  

Mirror speichert Daten gespiegelt.
Dadurch bleiben Daten auch bei einem Ausfall weiter erreichbar.
_________________________________
## Geplante Einstellungen
SimplePool (Disk04-06) - SimpleDisk - Simple (RAID0) 
MirrorPool (Disk07-09) - MirrorDisk - Mirror (RAID1) (2 Mirror und 1 Host-Spare)
ParityPool (Disk01-03) – ParityDisk - Parity (RAID5) 
_________________________________
## 1-1. Für „RAID (Mirror)“ - Physische Datenträger hinzufügen x3

## 1-2. Für „RAID (Mirror)“ - Speicherpool erstellen
```
Server Manager 
> Datei/Speicherdienste 
> Speicherpools 
> Aktualisieren 
> "Verfügbare Datenträger" auswählen 
> Aufgaben: Neu 
-> "MirrorPool" 
-> zwei Disks: Automatisch, eine Disk: "Host-Spare"
```

## 2. Paritätsvolume erstellen

```
2-1. "MirrorPool" auswählen 
> Virtuelle Datenträger 
> Aufgaben: Neu 
> "MirrorDisk" 
> (ohne) Gehäuseinformationen aktivieren 
> "Mirror" 
> Fest 
> Maximale
```
```
2-2. Neuer Assistent erscheint: 
Vorbemerkungen: Laufwerk "G:"
```

## 3. Redundanz prüfen
```
3-1. Server-Explorer 
> Laufwerk "G:" 
> neue Datei erstellen

3-2. Hyper-V 
> Server 
> ein Disk (Exklusive Hot-Spare-Disk) entfernen

3-3. Server-Manger 
> aktualisieren 
> "!"

3-4. Server neustart 
```
--> Zugriff auf die Dateien und Änderungen sind möglich. Das heißt, die `Redundanz (Spiegelung)` funktioniert korrekt.

## 4. Prüfung: automatische Reparaturfunktion (in Arbeit)
```
4-1. Physische Datenträger: 
Laufwerk "!" (Exklusive Hot-Spare-Disk) auswählen 
> entfernen

4-2. Hyper-V 
> Server 
> neu Disk hinzufügen

4-3. Server-Manger
> Physische Datenträger: Verwendung: "Hot-Spare" 
-> "Veraltet"
```
--> und dann? 18.05. 11:45-
