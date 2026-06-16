# RAID 5 ähnlich: Parity

Parity speichert Daten mit Paritätsinformationen.
Dadurch bleibt das System bei Ausfall eines Datenträgers weiter nutzbar.

## Kurzinfo

| Punkt | Wert |
|---|---|
| Pool | `ParityPool` |
| Virtueller Datenträger | `ParityDisk` |
| Layout | `Parity` |
| Laufwerk | `E:` |
| Datenträger | 3 x 10 GB |

## Geplante Struktur

| Pool | Disk | Typ |
|---|---|---|
| `ParityPool` | `ParityDisk` | Parity, ähnlich RAID 5 |
| `SimplePool` | `SimpleDisk` | Simple, ähnlich RAID 0 |
| `MirrorPool` | `MirrorDisk` | Mirror, ähnlich RAID 1 |

## Schritte

| Schritt | Ort | Aktion |
|---|---|---|
| 1 | Hyper-V | 3 physische Datenträger hinzufügen |
| 2 | Server-Manager | Speicherpool `ParityPool` erstellen |
| 3 | PowerShell | Virtuellen Datenträger `ParityDisk` erstellen |
| 4 | Server-Manager | Konfiguration prüfen |
| 5 | Datenträgerverwaltung | Datenträger initialisieren und Volume `E:` erstellen |
| 6 | Test | Datei anlegen und Festplattenausfall simulieren |
| 7 | Reparatur | Neue Festplatte hinzufügen und Reparatur starten |

## Kurzverfahren

1. In Hyper-V drei neue Festplatten mit je 10 GB hinzufügen.
2. Im Server-Manager den Speicherpool `ParityPool` erstellen.
3. Die Erstellung des Parity-Datenträgers über die GUI kann fehlschlagen.
4. In diesem Fall den virtuellen Datenträger per PowerShell anlegen.

```powershell
New-VirtualDisk `
  -StoragePoolFriendlyName "ParityPool" `
  -FriendlyName "ParityDisk" `
  -ResiliencySettingName Parity `
  -NumberOfColumns 3 `
  -ProvisioningType Fixed `
  -UseMaximumSize
```

5. In `diskmgmt.msc` den Datenträger neu einlesen, online schalten und mit `GPT` initialisieren.
6. Danach im Server-Manager ein neues Volume `E:` erstellen.
7. Im Explorer einen Ordner und eine Textdatei auf `E:` anlegen.
8. In Hyper-V eine Festplatte entfernen.
9. Im Server-Manager die Warnung `!` prüfen.
10. Zugriff auf `E:` testen.

## Ergebnis

- Wenn der Zugriff auf `E:` weiter möglich ist, arbeitet die Parität wie erwartet.
- Das System bleibt trotz eines Ausfalls betriebsbereit.
- Die Daten bleiben erhalten.

## Reparatur

1. In Hyper-V eine neue Festplatte hinzufügen.
2. Im Server-Manager den Datenträger dem Pool `ParityPool` hinzufügen.
3. Den virtuellen Datenträger `ParityDisk` reparieren.
4. Den fehlerhaften Datenträger entfernen.

Falls nötig:

```powershell
Get-PhysicalDisk | Where-Object { $_.OperationalStatus -ne "OK" } | Set-PhysicalDisk -Usage Retired
Repair-VirtualDisk -FriendlyName "ParityDisk"
Get-StorageJob
```

## Hinweis

Dieses Beispiel zeigt ein **Parity Layout**, das sich ähnlich wie **RAID 5** verhält.
Es verbindet Redundanz und bessere Kapazitätsnutzung.
