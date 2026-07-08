## Windows Server 2022 Installation
```
Virtueller Computer (VM) starten 
> "Press any Key" sofort bestätigen 
> Windows Server 2022 Standard Evaluation (Desktopdarstellung) 
> Benutzerdefiniert: Nur Microsoft Server-Betriebssystem installieren (Erweitert) 
> Weiter > Installation und automatischer Neustart 
-> (Administrator) Kennwort: „!Abcd1234“
```
\* Falls kein Login-Bildschirm (Passwort-Eingabefeld) -> (oben Menü) Aktion: STRG+ALT+ENTF(Ende))

### Windows Server 2022 Konfiguration
Anmelden > Server-Manager (Automatisch) >  Meldungen schließen und "Willkommen-" ausblenden

### Hostname konfigurieren
(linkes Menü) Localer Server > Computername (blau) klicken > "Ändern" -> Computername:SRV-DC01 > ok > schließen > jetzt neu starten

### IP-Address statisch konfigurieren
(linkes Menü) Localer Server > Ethernet (blau) klicken > Ethernet rechtsklick > Eigenschaften > IPv4 > Eigenschaften -> 
```
Folgende IP-Adresse
 192.168.10.10
 255.255.255.0
 192.168.10.1
Folgende DNS-Serveradressen
 192.168.10.10 
```
\> ok > schließen

###  Hinzufügen der Funktionen: AD (Aktive Directory), DHCP und DNS
(im oberen Menü) Verwalten > Rollen und Features hinzufügen > weiter > Rollenbasierte oder featurebasierte Installation > Einen Server aus dem Serverpool auswählen >  
Serverrollen:  
 1\. Active Directory-Domänendienste (-> hacken -> Features hinzufügen)  
 2\. DHCP-Server  
 3\. DNS-Server  
\> weiter > installieren > (war erfolgreich) > schließen

### AD (Aktive Directory) konfigurieren
(im oberen Menü) Warnflagge mit "!" klicken > Server zu einem Domänencontroller heraufstufen > „Neue Gesamtstruktur hinzufügen“ anhacken; Name der Stammdomäne: „firma.local“ > (Vorgang läuft, blauer Balken) > Kennwort: „!Abcd1234“ (für Notfall-Passwort) > weiter > DNS Option: kein Haken > (automatisch ausgefüllt) Der NetBIOS-Domänenname: „FIRMA“ > weiter -> installieren -> automatischer Neustart

### DHCP konfigurieren
(im oberen Menü) Warnflagge mit "!" klicken > DHCP-Konfiguration abschließen > weiter > (automatisch ausgefüllt) Commit ausführen > schließen

### DHCP einstellen 1
(im oberem Menü) Tools > DHCP > firma.local > IPv4 rechtsklick: Neuer Bereich > "Name" > Start-IP:192.168.10.100, End-IP: 192.168.10.199 > Ausschlüsse und Verzögerung hinzufügen: nix  > Leasedauer: egal > DHCP-Optionen: ja > Router(Standardgateway): 192.168.10.1 > Domänenname und DNS-Server: (Wird automatisch ausgefüllt) firma.local und IP-Address (Falls die IP-Addresse nicht erscheint, Servernamen eingeben > „Auflösen“ > IP-Adresse erscheint > „Hinzufügen“) > WINS-Server: nix > Bereich aktivieren: ja > Fertig stellen

\* WINS: Nur für Uralt-Apps (NetBIOS). Rest: Immer DNS.

### DHCP einstellen 2
Tools > DHCP > firma.local > IPv4 rechtsklick: Eigenschaften > DNS-Reiter > DNS Einträge immer dynamisch aktualisieren > Übernehmen > ok

------------------------------------------------
## Domänenbenutzer: Erstellung und Konfiguration 

### Organisationseinheit (OU) in Active Directory erstellen
Tools > Active Directory Benutzer und Computer > „firma.local“ rechtsklick > Neu > Organisationseinheit -> Name > ok

\* Wenn OU und Gruppe den gleichen Namen haben, muss man zuerst die OU erstellen.

### Neuen Benutzer in OU erstellen
OU "IT" rechtsklick > neu > Benutzer > Vorname, Nachname, Benutzeranmeldename (z.B. edward snowden; e.snowden@firmal.local entspricht der Login-ID) > weiter > Kennwort eingeben und "Kennwort läuft nie ab" hacken (und Fehlermeldung ok) > Fertig stellen

### Beschränkungen für Benutzerpasswort deaktiviert
Tools > Gruppenrichtlinienverwaltung > Gesamtstruktur > Domänen > firma.local > Default Domain Policy rechtsklick: bearbeiten  

Computerkonfiguration > Richtlinien > Windows Einstellungen > Sicherheitseinstellungen > Kontorichtlinien > Kennwortrichtlinien >  
 1\. Kennwort muss Komplexitätsvoraussetzungen entsprechen: deaktiviert  
 2\. Minimale Kennwortlänge: z.B. 1 Zeichen  
aktualisieren: Eingabeaufforderung (CMD) -> gpupdate /force

### Admin-Rechte vergeben
Benutzer rechtsklick > Eigenschaften > Mitglied von: „Dömanen-Admin“  
 Domänen-Admin: Für alle PCs und Server in der Domäne  
 Administrator: Nur für diesen Server  
