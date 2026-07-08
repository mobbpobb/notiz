## Windows 11 Installation

### Voraussetzungen: Fehlermeldung „PC muss TPM 2.0 unterstützen“ umgehen
Virtueller Maschine (VM) rechtsklick > Sicherheit > "Trusted Platform Module" aktivieren > Anwenden > ok
 
### Installieren
VM starten > "Press any Key" sofort bestätigen  -> „Ich habe keinen Product Key“ -> Windows 11 Pro -> weiter -> 
\1. Mit Internet: > Ein Konto wird gefordert: Den VM-Switch kurz löschen, um offline zu gehen. Nicht vergessen, den Switch wieder zurückzusetzen.
\2. Ohne Internet: > "Lassen Sie sich mit einem Netzwerk verbinden" : Shift + F10 (Es öffnet sich die Eingabeaufforderung (CMD) mit Administratorrechten) > oobe\bypassnro > weiter > „Ich habe kein Internet“ Option erscheint > 
"Name" (das lokale Benutzerkonto erhält automatisch Administratorrechte) > Kennwort > Sicherheitsfrage (1-3) > Nein oder nur Erforderlich ->

### Domänenbeitritt
\* Wenn die IP-Adresse nicht wie unten angegeben ist manuelle IP-Konfiguration des Clients (Windwos 11) erforderlich, damit sich dieser im gleichen Netzwerksegment wie der Domänencontroller (Windwos Server 2022) befindet.

Win+R -> ncpa.cpl -> Der betreffende Netzwerkadapter > Eigenschaften > IPv4
 192.168.10.105	ip
 255.255.255.0	sub
 192.168.10.1		gateway
 192.168.10.10 	dns

Windows 11 > Start > Einstellungen > System > Info > Domäne oder Arbeitsgruppe > Ändern > Domäne: „firma.local“ > (Authentifizierung für den Domänenbeitritt) Benutzername: Administrator, Kennwort: „!Abcd1234“ > "Willkommen in der Domäne" > ok > ok > schließen > popup: jetzt neu starten

**Prüfen**: Anmeldung mit dem Domänenbenutzer: e.snowden (falls das nicht geht: e.snowden@firma.local), der im Active Directory angelegt wurde.

**nicht vergessen**: die IP‑Adresse wieder auf DHCP umstellen.

\* Wenn du dich mit einem lokalen Konto anmelden möchtest, Benutzername: „.\localname“.

\* Hinweis: Benutzererstellung via PowerShell
\1. Das Passwort wird nicht direkt im Befehl mitgegeben, sondern erst nach der Ausführung des Befehls eingegeben. 
\2. Der Benutzer wird aus der Standard-Benutzergruppe in die entsprechende OU verschoben. 
