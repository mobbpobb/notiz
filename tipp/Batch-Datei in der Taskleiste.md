Einfache Methode mit Verknüpfung
So kannst du eine .bat oder .cmd an die Taskleiste hängen:

Batch-Datei an einen festen Ort legen (z.B. C:\Scripts\meinskript.bat).

Rechtsklick auf die Batch-Datei → „Weitere Optionen anzeigen“ → „Verknüpfung erstellen“. Die Verknüpfung liegt dann z.B. auf dem Desktop.

Rechtsklick auf die Verknüpfung → „Eigenschaften“.

Im Feld „Ziel“ ganz vorne einfügen:
C:\Windows\System32\cmd.exe /c start "" "<PfadZurBatchDatei>"
Beispiel:
C:\Windows\System32\cmd.exe /c start "" "C:\Scripts\meinskript.bat"

„Übernehmen“ und „OK“.

Jetzt Rechtsklick auf die Verknüpfung → „Weitere Optionen anzeigen“ → „An Taskleiste anheften“ (oder Verknüpfung mit der Maus in die Taskleiste ziehen).

Ab jetzt startet ein Klick auf das Taskleisten-Icon deine Batch-Datei.