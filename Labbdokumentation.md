# Labbdokumentation
Namn: Isman Bishaar Mahamud

Datum: 2026-09-14

Kurs: Introduktion till yrkesrollen och grunderna i IT-infrastruktur (MYH 2025/4008)

## Introduktion

Denna dokumentation beskriver uppsättningen av en virtuell labbmiljö med en Linux-server och en Windows-klient. Dokumentationen innehåller även kommandoradsarbete i Linux och Windows, Git-versionering samt en kritisk reflektion kring användningen av generativ AI.

## Labbmiljö & Nätverk

Labbmiljön består av två virtuella maskiner som körs i UTM. Linux-servern och Windows-klienten är anslutna till samma Host-Only-nätverk, `Network 0`, så att de kan kommunicera med varandra.

| Hostname | Operativsystem | IP-adress | Subnätmask | Standard Gateway |
|---|---|---|---|---|
| `linux-server` | Linux/Ubuntu | `192.168.1.50` | `255.255.255.0` | Ingen |
| `WIN-2PRLUV1NOVH` | Windows 11 | `192.168.1.51` | `255.255.255.0` | Ingen |

Linux använder den statiska IP-adressen `192.168.1.50/24` och Windows använder `192.168.1.51/24`. Båda maskinerna ligger därför i samma subnät.

Kommunikationen testades i båda riktningarna. Från Linux användes `ping 192.168.1.51` och från Windows användes `Test-Connection 192.168.1.50`. Båda testerna lyckades.

Kommandoradsgenomförande 
Linux – Bash
Katalogen /var/systementor/konsultdata skapades via kommandoraden:
sudo mkdir -p /var/systementor/konsultdata
Filen anteckningar.txt skapades:
sudo touch /var/systementor/konsultdata/anteckningar.txt
Gruppen konsulter skapades:
sudo groupadd konsulter
Katalogen och filen tilldelades gruppen konsulter:
sudo chgrp konsulter /var/systementor/konsultdata
sudo chgrp konsulter /var/systementor/konsultdata/anteckningar.txt
Behörigheterna sattes enligt principen om lägsta behörighet:
sudo chmod 750 /var/systementor/konsultdata
sudo chmod 640 /var/systementor/konsultdata/anteckningar.txt
Behörigheterna kontrollerades med:
sudo ls -la /var/systementor/konsultdata
Nätverksanslutningen till Windows-VM verifierades med:
ping 192.168.1.51
Linux-serverns nätverkskort kontrollerades med:
ip addr show
Nätverkskortet enp0s1 hade IPv4-adressen 192.168.1.50/24 och var aktivt.
Windows – PowerShell
Mappen C:\Systementor\KonsultData skapades via PowerShell:
New-Item -ItemType Directory -Path "C:\Systementor\KonsultData" -Force
Behörighetsstrukturen för mappen kontrollerades med:
Get-Acl "C:\Systementor\KonsultData"
Nätverksanslutningen till Linux-VM verifierades med:
Test-Connection 192.168.1.50
Windows nätverksinställningar kontrollerades med:
ipconfig /all