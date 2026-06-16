# RAID 1 ähnlich: Mirror

Mirror speichert Daten gespiegelt.
Dadurch bleiben Daten bei einem Ausfall weiter verfügbar.

## 

1. Im Server-Manager unter **Datei- und Speicherdienste > Speicherpools** den Pool `MirrorPool` erstellen.
2. Zwei Disks normal verwenden und eine Disk als `Hot-Spare` lassen.
3. Unter **Virtuelle Datenträger** den Datenträger `MirrorDisk` mit Layout `Mirror` erstellen.
4. Im Assistenten das Laufwerk `G:` anlegen.
5. Eine oder mehrere Testdateien auf `G:` speichern.
6. In Hyper-V eine aktive Festplatte entfernen, nicht die Hot-Spare-Disk.
7. Im Server-Manager die Warnung `!` prüfen und den Server neu starten.

## Ergebnis

- Der Zugriff auf die Dateien bleibt möglich.
- Änderungen auf `G:` sind weiter möglich.
- Die Spiegelung funktioniert damit grundsätzlich korrekt.

## Offener Punkt

Der Reparaturteil ist noch nicht vollständig abgeschlossen.
Bisher ist notiert:

1. Fehlerhaften physischen Datenträger entfernen.
2. Neue Festplatte in Hyper-V hinzufügen.
3. Status im Server-Manager erneut prüfen.

## Hinweis

Dieses Beispiel zeigt ein **Mirror Layout**, das sich ähnlich wie **RAID 1** verhält.
Hier steht die Redundanz im Vordergrund.
