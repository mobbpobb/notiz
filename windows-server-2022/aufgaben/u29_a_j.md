## a. Der Kunde wünscht ein Power Shell Skript für das Anlegen neuer MA. 

a). Es soll ein sicheres einheitliches Aktivierungspasswort genutzt werden.  
b). Das Skript soll eine CSV-Datei im gleichen Ordner einlesen.  
c). Name der Datei MA_Neu.csv  
d). Neue Mitarbeiter sollen automatisch im AD der richtigen Abteilung und Führungsebene zugeordnet werden.  
e). Punkt a bis d stellen jeweils Erweiterungen dar und müssen nicht gemacht werden, um ein Skript für den Kunden zu haben.  
f). Achtung bei KI-Nutzung. Das Skript muss erklärt werden können  

**Abruf aktueller AD-User-Infos und Export als CSV**    
cmd > powershell_ise.exe ise > Get-ADUser -Filter * -Properties * | Export-CSV aduser_export2.csv -Encoding utf8 -Delimiter ","

**Erstellung der CSV-Datei für den Benutzerimport**  
--> import_user.csv

Name,Path,GivenName,DisplayName,SamAccountName,Surname,UserPrincipalName,Groups 
DavidSchneider,"OU=03_Abteilungsleiter,OU=03_Marketing,OU=U29,DC=firma,DC=local",David,David Schneider,d.schneider,Schneider,d.schneider@firma.local,"03_Marketing,03_Abteilungsleiter"

**Erstellung des PowerShell-Skripts**  
--> add_users.ps1

1\. Einrichten des Initialpassworts  
$SecurePassword = ConvertTo-SecureString "1" -AsPlainText -Force

2\. CSV-Datei importieren  
```
Import-Csv -Path ".\import_users.csv" -Delimiter "," -Encoding UTF8 | ForEach-Object {
  #3. Neuen Benutzer anlegen
	New-ADUser -Name $_.Name `
           -SamAccountName $_.SamAccountName `
	           -UserPrincipalName $_.UserPrincipalName `
	           -DisplayName $_.DisplayName `
	           -GivenName $_.GivenName `
	           -Surname $_.Surname `
	           -Path $_.Path `
	           -AccountPassword $SecurePassword `
	           -ChangePasswordAtLogon $true `
	           -Enabled $true

  #1. Gruppen-String trennen und in ein Array (Liste) umwandeln
  $GroupList = $_.Groups -split ","
  #2. Jede Gruppe einzeln nacheinander abarbeiten (Schleife)
  foreach ($Group in $GroupList) {
  #3. Leerzeichen entfernen und Benutzer der AD-Gruppe hinzufügen
  Add-ADGroupMember -Identity $Group.Trim() -Members $_.SamAccountName

}
}
```
**Ausführung des Skripts**  
Vor der Ausführung: OU und Gruppen müssen erstellt werden.  
Powershell > .\add_users.ps1

**Prüfen Überprüfung im Active Directory Login-Prüfung unter Win11**  
 --> OK (Passwortänderung bei der ersten Anmeldung)  
Fehler: Hintergrundbild wird nicht angewendet -> Die 03_Gruppen waren nicht Mitglied der U29-Gruppe.

_________________________________
## b. Einbindung eines Netzlaufwerkes zur Speicherung von Daten für Teamleiter aufwärts.  (Es reichen Symbolische 500MB)  

a). Einbindung erfolgt dynamisch beim Login unabhängig vom Gerät

1\. Ordnerberechtigungen einrichten

*\ Bereits im Schritt „9. Hintergrund“ durchgeführt  
"\Desktop\Aufgabe_U29\" > Eigenschaften >  
> Reiter:Freigabe > Erweiterte Freigabe > check "Diesen Ordner freigeben", Berechtigungen > Hinzufügen > Jeder:"Vollzugriff"  
> Reiter:Sicherheit > Bearbeiten > Hinzufügen > "U29":"Lesen"(x3)  

Server > neuer Ordner "\Desktop\Aufgabe_U29\Netzlaufwerk_U29\" erstellen > Eigenschaften > Sicherheit > Erweitert >  
> "Vererbung deaktivieren" > "Vererbe Berechtigungen in explizite... konvertieren" >  
> Entfernen: Gruppen für normale Mitarbeiter und alle Mitarbeiter  
> Hinzufügen: Prinzipal auswählen > 02_Abteilungsleiter, 02_Teamleiter > Vollzugriff  

2\. Kontingent von 500 MB für den Ordner einrichten  

\* Ressourcen-Manager für Dateiserver muss installieren  

Server > Tools > Ressourcen-Manager für Dateiserver > Kontingentverwaltung > Kontingente > Kontingent erstellen... > Kontingentpfad: Durchsuchen > \Desktop\Aufgabe_U29\Netzlaufwerk_U29\ > Benutzerdefiniertes Kontingent erstellen > 500 MB, "Harte Kontingentgrenze"

3\. Netzlaufwerke per GPO zuweisen  
GPO > U29 > Computers > neues Objekt erstellen > das Objekt Bearbeiten > Benutzerkonfiguration > Einstellungen > Windows-Einstellungen> Laufwerkszuordnungen > Neu > Zugeordnetes Laufwerk > 
> Aktion: Erstellen
> Speicherort: \\SRV-DC01\Aufgabe_U29\Netzlaufwerk_U29
> Beschriften als: NetzlaufwerkU29
> Laufwerkbuchstabe:Verbenden: N

Reiter-Gemeinsame Optionen > Zielgruppenadressierung auf Elementebene > Zielgruppenadressierung.. > 
> Neues Element > Sicherheitsgruppe > (Gruppe) ... > "02_Teamleiter" > 
> Neues Element > Sicherheitsgruppe > (Gruppe) ... > "02_Abteilungsleiter" > 
> Menü: Elemetoptionen --> "Oder" (Wichtig!)  

**Prüfen**: Win11 > Dieser PC -> NetzlaufwerkU29 (N:) wird angezeigt


\* GPO-Verschiebung (Move):  
GPO löschen -> Neue Ziel-OU > Vorhandenes Gruppenrichtlinienobjekt verknüpfen... > GPO auswählen

\* GPO-Löschung (Delete):  
GPO > firma.local > Gruppenrichtlinienobjekte > GPO-Originale aus der Liste löschen.

_________________________________
## c. Einblendung einer Motivierenden Nachricht beim Login. 

GPO > U29 > Computers > neues Objekt erstellen > das Objekt Bearbeiten > Computerkonfiguration > Richtlinien > Windows-Einstellungen > Sicherheitseinstellungen > Lokale Richtlinien > Sicherheitsoptionen

Interaktive Anmeldung: Nachrichtentext für Benutzer, die sich anmelden möchten
-> Willkommen bei der U29 GmbH!

Interaktive Anmeldung: Nachrichtentitel für Benutzer, die sich anmelden möchten
-> Gemeinsam erreichen wir heute unsere Ziele! Auf einen erfolgreichen Arbeitstag.

Prüfen: Eingerichtete Meldung erscheint vor dem Login

_________________________________
## d. Aktivieren der Bitlocker Verschlüsselung
 a). Mit Eingabe eines Pins (6-Stellig) vor dem Bootvorgang. 
 b). Speicherung des Schlüssels im AD. 

Hyper-V > Windows 11 > SCSI > DVD entfernen

Windows11 > gpedit.msc > Computer > Admin > Windows > BitLocker > Betriebssstem > 
 Minimale Pin.. aktiviert (4)1
 Verwendung der BitLocker.. aktiviert
 Zusätzliche Authentifizierung.. aktiviert

Windows11 > C: > BitLocker aktivieren > 
 PIN eingeben:1234
 Wiederhertellungsschlüssel drucken > "Microsoft Print to PDF" drucken -> PDF-Datei erstellt > weiter > neu starten --> Wird ein Passwort angefordert?

Diese PDF-Datei in das Netzlaufwerk kopieren 

_________________________________
## e. Einführen eines 2ten Faktors für Führungskräfte

--> Geräte für die Aufgabe erforderlich?

_________________________________
## f. Aktivierung des Windows Defenders über die AD

GPO > U29 > Computers > neue GPO > Computer > Richtlinien > Admin > Windows-Komponenten > Microsoft Defender Antivirus > 
Microsoft Defender Antivirus deaktivieren > Deaktiviert

GPO > U29 > Computers > neue GPO > Computer > Richtlinien > Windows-Einstellungen > Sicherheitseinstellungen > Windows Defender Firewall mit erweiterter Sicherheit > Windows Defender Firewall mit erweiterter Sicherheit - LDAP.. > Eigenschaften > 
Für alle Reiter:
 Firewallzustand: Ein (empfohlen)
 Eingehende Verbindungen: Blockieren (Standard)

Prüfen: Win11 > Windows Defender Wall -> 
1. erscheint "Zu Ihrer Sicherheit werden einige Einstellungen vom Systemadministrator verwaltet"
2. keine Änderungen möglich, außer für Benutzer mit Admin-Rechten

---------------------------------------
// g. Aktivierung der Windows Firewall über die AD

a). Nutzung von HTTP soll verboten sein. 
b). POP 3 soll verboten sein 
c). Wichtig IPv4 und IPv6 müssen nicht zwingend konfiguriert sein aber berücksichtigt 

// a).- b). HTTP->TCP:80, POP3->TCP:110
GPO > U29 > Computers > neue GPO > Computer > Richtlinien > Windows-Einstellungen > Sicherheitseinstellungen > Windows Defender Firewall mit erweiterter Sicherheit > Windows Defender Firewall mit erweiterter Sicherheit - LDAP.. > Ausgehende Regeln > Neue Regel > Port > TCP:80 > "Verbindung blockieren" > check alle "Domäne, Privat, Öffentlich" > Name

a). HTTP Prüfen: Web Browser > http://neverssl.com -> Fehler: blockiert
b). POP3 Prüfen: PowerShell> 
Test-NetConnection -ComputerName outlook.office365.com -Port 110
-> TcpTestSucceeded : False

---------------------------------------
//h. Jeder Mitarbeiter soll über einen Ordner auf dem Desktop verfügen mit einer Netzwerkfreigabe.  

 a). Der Zugriff auf den Ordner soll ausschließlich über die Login Daten des MA möglich sein. 

* für t.müller von Mitarbeiter-Gruppe

1. Ordner erstellen
C:\Users\Administrator\Desktop\Aufgabe_U29\Share\02_Vertrieb\02_Mitarbeiter\t.müller

2. GPO Verknüpfungen
neu GPO unter 02_Mitarbeiter > Benutzer > Einstellungen > Windows >  Verknüpfungen
neu > Verknüpfung > 
Reiter-Allgemein > Aktualisieren > Nanme:02_Mitarbeiter > Dateisystemobjekt > Desktop > "\\SRV-DC01\Aufgabe_U29\Share\02_Vertrieb\02_Mitarbeiter\%USERNAME%"
Reiter-Gemeinsame > Zielgruppen > 02_Mitarbeiter

3. Ordner-Recht
\Share\ > 
Freigabe: check > Share$ ($=Versteckte Freigaben) > Berechtigungen U29 -> Vollzugriff > ok > 
Sicherheit: U29 vollzugriff

\Share\02_Vertrieb\ >
Sicherheit: 02_Vertrieb lesen, U29 löschen

\Share\02_Vertrieb\02_Mitarbeiter\t.müller\ > 
Sicherheit: Erweitert > Vererbung deaktivieren > ..in explizite.. > 
t.müller vollzugriff, 02_Vertrieb löschen

4. Prüfen


 b). Der Ordner soll als Ziel für Dokumenten Scans über das Netzwerk genutzt werden. Quelle Gerät Multifunktionsdrucker. 

--> kein Multifunktionsdrucker

---------------------------------------
//i. Beim Login des Mitarbeiters soll der Drucker direkt automatisch im Gerät hinterlegt werden.  

* Da eine Remoteverbindung aktiv ist, wird der Drucker umgeleitet. Der Drucker muss lokal auf dem Server installiert sein.

Server > Systemstarten > Hardware > Geräte und Drucker > Drucker hinzufügen
erstellter Drucker > Druckereigenschaften > Freigabe > check "Drucker freigaben", Freigabename: DruDru

GPO > GPO für alle Mitarbeiter auswählen (z.B.:aufgabe_4_shutdown) > Bearbeiten > Einstellungen > Systemsteuer > Drucker
neu > Freigegebener Drucker > Aktion: Aktualisieren, Freigegebener Drucker: pfad: \\SRV-DC01\DruDru > check "Drucker als Standarddrucker fest.."

Win11 cmd > gpupdate /force
Win11 start > Einstellungen > Geräte > Drucker und Scanner -> Erscheint der Drucker automatisch(nach einigen Sekunden)

---------------------------------------
//j. Die Nutzung der Internetseite FACEBOOK.de und aller anderen Facebook Domänen soll unterbunden werden.

Server > Tools > DNS > Forward-Lookupzonen > Neue Zone.. > Primäre Zone > Zonenname: facebook.de
ein weitere Neue Zone einrichten (Zonenname: facebook.com)

Win11 browser > suche: facebook > klick auf den Link > Fehlermeldung (Zugriff verweigert)
