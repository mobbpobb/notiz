
# DFS-Namespace-Synchronisationsfehler

Der neu erstellte Namespace wird nicht angezeigt, während der alte Namespace weiterhin als fehlerhaftes „Ghost-Objekt“ (Element nicht gefunden) die Ansicht blockiert.

## Grund: 

Ein Replikationsstau im Active Directory (AD). Bei schnellen Änderungen in der DFS-Konsole wird die AD-Datenbank nicht rechtzeitig synchronisiert. Die Konsole verbleibt in einem undefinierten Zustand und blockiert die Anzeige des neuen Namespaces.

## Lösungsschritte
```
1. Konsolen-Bereinigung
Alten Namespace (\\firma.local\public) rechtsklicken 
> „Namespace aus Anzeige entfernen“ wählen (löscht nur die Ansicht, nicht die Daten).

2. Dienst-Neustart (Cache löschen)
"services.msc" öffnen  
-> Dienst „DFS-Namespaces“ suchen 
> „Neu starten“ wählen, um die AD-Synchronisation zu erzwingen.

3. Namespace wiederherstellen
Ordner „Namespaces“ rechtsklicken 
> „Namespaces zur Anzeige hinzufügen...“ wählen
> Domäne "firma.local" abfragen, den neuen Namespace auswählen und mit „OK“ wieder einblenden.
```