# Speicherpool & RAID 1 (Mirror)  

Mirror speichert Daten gespiegelt.
Dadurch bleiben Daten auch bei einem Ausfall weiter erreichbar.

## 1. Für „RAID (Mirror)“ - Speicherpool erstellen
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

## 3. Prüfen
```
3-1. Server Explorer 
> Laufwerk "G:" 
> neue Datei erstellen

3-2. Hyper-V 
> Server 
> ein Disk (Exklusive Hot-Spare-Disk) entfernen

3-3. Server-Manger 
> aktualisieren 
> "!"

3-4. Server Neustart 
```
--> Zugriff auf die Dateien und Änderungen sind möglich. Das heißt, die *Redundanz (Spiegelung)* funktioniert korrekt.

## 4. Reparatur?
```
4-1. Physische Datenträger: 
Laufwerk "!" (Exklusive Hot-Spare-Disk) auswählen 
> entfernen

4-2. Hyper-V 
> Server 
> neu Disk hinzufügen

4-3. Server-Manger 
> aktualisieren 
> Physische Datenträger: Verwendung: "Hot-Spare" 
-> "Veraltet"
```
--> und dann? 18.05. 11:45-
