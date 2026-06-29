# Speicherpool & RAID 5 (Parity)  

Parity speichert Daten mit Paritätsinformationen.
Dadurch bleibt das System bei Ausfall eines Datenträgers weiter nutzbar.
_________________________________
## Geplante Einstellungen
SimplePool (Disk04-06) - SimpleDisk - Simple (RAID0) 
MirrorPool (Disk07-09) - MirrorDisk - Mirror (RAID1) (2 Mirror und 1 Host-Spare)
ParityPool (Disk01-03) – ParityDisk - Parity (RAID5) 
_________________________________
## 1. für „RAID(Parity)” - Physische Datenträger hinzufügen x3
```
Hyper-V 
> Server Einstellung 
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
-> "ParityPool" 
-> Alle drei Datenträger auswählen 
```

## 3. Paritätsvolume erstellen
* `Fehler` (Erstellung über die GUI ist fehlgeschlagen):
```
Server-Manager 
> aktualisieren 
> "ParityPool" auswählen > 
Virtuelle Datenträger: Aufgaben: Neu 
> "ParityPool" auswählen 
> 
Name:"ParityDisk" 
> (ohne) Gehäuseinformationen aktivieren 
> "Parity" 
> Fest 
> Maximale
```

* `Stattdessen`: Erstellung über die CLI (PowerShell):
```
PowerShell (admin) 
>New-VirtualDisk `
  -StoragePoolFriendlyName "ParityPool" `
  -FriendlyName "ParityDisk" `
  -ResiliencySettingName Parity `
  -NumberOfColumns 3 `
  -ProvisioningType Fixed `
  -UseMaximumSize
```

## 4. Konfiguration prüfen
```
Server-Manager 
> aktualisieren 
> Die Laufwerksnamen erscheinen unter „Virtuelle Datenträger“ und „Physische Datenträger“
```

## 5. Laufwerkszuweisung und Datenerstellung
```
Server-Start 
> diskmgmt.msc (Datenträgerverwaltung) suchen
> Aktion 
> neu einlesen 
> 
Unten erscheint der Datenträger als „Nicht zugeordnet“ (schwarzer Balken) 
1. rechtsklick > Online
2. rechtsklick > Datenträgerinitialisierung > (GPT)
```
```
Server-Manager 
> aktualisieren 
> "ParityDisk" auswählen 
> Aufgaben:"Neues Volume" 
-> weiter und erstellen (E:)
```
```
Server-Explorer > "E:" 
-> Neuen Ordner und eine Textdatei erstellen
```

## 6. Paritätsprüfung (Simulation eines Hardware-Ausfalls)
```
Hyper-V 
> Server 
-> eine Festplatte entfernen

Server-Manger 
> aktualisieren 
> Das Laufwerk wird mit einem Warnsymbol (!) angezeigt

Server-Explorer 
-> Ist „E:“ vorhanden? Kannst du darauf zugreifen, Daten einsehen oder welche hinzufügen?
```

Falls ja –> 
Fazit: Dank Parität `bleibt das System trotz eines Festplattenausfalls betriebsbereit` und die Daten bleiben erhalten.

* Hinweis: Virtuelle Festplatten (.vhdx) können auch nach dem Löschen der VM im Hyper-V-Manager im Speicherordner verbleiben.
C:\ProgramData\Microsoft\Windows\Virtual Hard Disks

## 7. Reparatur und Aufheben der Warnmeldung (!)
```
Hyper-V 
> Server 
> eine Festplatte hinzufügen

Server-Manger 
> aktualisieren 
> Speicherpools:"ParityPool" 
> Physischen Datenträger hinzufügen 

Server-Manger 
> aktualisieren 
> Virtuelle Datenträger:"ParityDisk" 
> reparieren

Server-Manger 
> aktualisieren 
> Physische Datenträger:"Laufwerk(!)" 
> entfernen 
-> Warnsymbol(!) verschwindet
```

* Falls es nicht funktioniert:
```
Powershell>
Get-PhysicalDisk | Where-Object { $_.OperationalStatus -ne "OK" } | Set-PhysicalDisk -Usage Retired
Repair-VirtualDisk -FriendlyName "ParityDisk"
Get-StorageJob
```

```
Powershell>
Get-PhysicalDisk | Where-Object { \$_.OperationalStatus -ne "OK" } | Set-PhysicalDisk -Usage Retired
Repair-VirtualDisk -FriendlyName "ParityDisk"
Get-StorageJob
```
