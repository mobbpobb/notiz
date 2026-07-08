## Hyper-V aktivieren
Windows 11 > System > Suche > Windwos-Features aktivieren oder deaktivieren > Hyper-V anhaken > neu start

## Virtuelle Switch erstellen
Ohne virtuellen Switch hat die VM keine Hardware-Schnittstelle zum Kommunizieren. Er ist das digitale Gegenstück zum LAN-Kabel.

Hyper-V > (rechtes Menü) Manager für virtuelle Switches > Intern/Extern > Virtuellen Switch erstellen ->"Name" > anwenden > ok
„Intern“: Netzwerk nur zwischen Host und VMs, ohne Internet.
„Extern“: Verbindung zum physischen Netzwerk, z.B. Firmennetz oder Internet.
„Privat“: Netzwerk nur zwischen den VMs, ohne Host und ohne Internet.

## Virtueller Maschine (VM) einstellen (für Windows Server 2022 und Windows 11)
Hyper-V > neu > weiter -> "Name" > „Generation 2“ (UEFI) -> "8192"MB -> Verbindung: " den oben erstellten Switch" -> Größe "80"GB > (zweite Option) Betriebssystem von einer startbaren Imagedatei installieren -> Durchsuchen -> „ISO-Datei“ -> Öffnen > weiter > Fertig stellen

\* virtuellen Festplattendateien (.vhdx) gespeichert
(z.B.) C:\ProgramData\Microsoft\Windows\Virtual Hard Disks

## VM‑Einstellungen
1\. Prüfpunkte > „Prüfpunkte aktivieren“ aushacken  
2\. Automatische Starkaktion > Keine Aktion
