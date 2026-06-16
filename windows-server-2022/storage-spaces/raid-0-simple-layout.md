# RAID 0 ähnlich: Simple Layout

Simple Layout verteilt Daten auf mehrere Datenträger.
Es ist schnell, aber es gibt **keine Redundanz**.

## Kurzinfo

| Punkt | Wert |
|---|---|
| Pool | `SimplePool` |
| Virtueller Datenträger | `SimpleDisk` |
| Layout | `Simple` |
| Laufwerk | `F:` |
| Datenträger | 3 x 10 GB |

## Schritte

| Schritt | Ort | Aktion |
|---|---|---|
| 1 | Hyper-V | 3 physische Datenträger hinzufügen |
| 2 | Server-Manager | Speicherpool `SimplePool` erstellen |
| 3 | Server-Manager | Virtuellen Datenträger `SimpleDisk` mit Layout `Simple` erstellen |
| 4 | Assistent | Laufwerk `F:` anlegen |
| 5 | Test | Datei speichern und einen Datenträger entfernen |

## Kurzverfahren

1. In Hyper-V drei neue Festplatten mit je 10 GB hinzufügen.
2. Im Server-Manager unter **Datei- und Speicherdienste > Speicherpools** den Pool `SimplePool` erstellen.
3. Unter **Virtuelle Datenträger** einen neuen Datenträger mit dem Namen `SimpleDisk` und dem Layout `Simple` erstellen.
4. Im Assistenten das Laufwerk `F:` anlegen.
5. Eine Testdatei auf `F:` speichern.
6. In Hyper-V eine Festplatte entfernen.
7. Im Server-Manager die Warnung `!` prüfen.

## Ergebnis

- Der Zugriff auf `F:` ist zuerst oft noch möglich.
- Nach einem Neustart ist der Zugriff auf `F:` nicht mehr möglich.
- Eine Reparatur ist hier nicht wie bei Parity vorgesehen.

## Hinweis

Dieses Beispiel zeigt ein **Simple Layout**, das sich ähnlich wie **RAID 0** verhält.
Es ist nur für Übung oder zum Verstehen des Verhaltens gedacht.
