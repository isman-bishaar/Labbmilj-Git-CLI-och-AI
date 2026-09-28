# Labbdokumentation
Namn: Isman Bishaar Mahamud

Datum: 2026-09-14

Kurs: Introduktion till yrkesrollen och grunderna i IT-infrastruktur (MYH 2025/4008)

## Introduktion

Denna dokumentation beskriver uppsättningen av en virtuell labbmiljö med en Linux-server och en Windows-klient. Dokumentationen innehåller även kommandoradsarbete i Linux och Windows, Git-versionering samt en kritisk reflektion kring användningen av generativ AI.

## Labbmiljö & Nätverk (kursmål 8)

Labbmiljön består av två virtuella maskiner som körs i UTM. Linux-servern och Windows-klienten är anslutna till samma Host-Only-nätverk, `Network 0`, så att de kan kommunicera med varandra.

| Hostname | Operativsystem | IP-adress | Subnätmask | Standard Gateway |
|---|---|---|---|---|
| `linux-server` | Linux/Ubuntu | `192.168.1.50` | `255.255.255.0` | Ingen |
| `WIN-2PRLUV1NOVH` | Windows 11 | `192.168.1.51` | `255.255.255.0` | Ingen |

Linux använder den statiska IP-adressen `192.168.1.50/24` och Windows använder `192.168.1.51/24`. Båda maskinerna ligger därför i samma subnät.

Kommunikationen testades i båda riktningarna. Från Linux användes `ping 192.168.1.51` och från Windows användes `Test-Connection 192.168.1.50`. Båda testerna lyckades.

## Kommandoradsgenomförande (Kursmål 9) 

 ### Linux – Bash
Katalogen /var/systementor/konsultdata skapades via kommandoraden:

```bash
sudo mkdir -p /var/systementor/konsultdata
```
Filen anteckningar.txt skapades:

```bash
sudo touch /var/systementor/konsultdata/anteckningar.txt
```
Gruppen konsulter skapades:

```bash
sudo groupadd konsulter
```
Katalogen och filen tilldelades gruppen konsulter:

```bash
sudo chgrp konsulter /var/systementor/konsultdata

sudo chgrp konsulter /var/systementor/konsultdata/anteckningar.txt
```
Behörigheterna sattes enligt principen om lägsta behörighet:

```bash
sudo chmod 750 /var/systementor/konsultdata

sudo chmod 640 /var/systementor/konsultdata/anteckningar.txt
```
Behörigheterna kontrollerades med:

```bash

sudo ls -la /var/systementor/konsultdata
```
Nätverksanslutningen till Windows-VM verifierades med:

```bash
ping 192.168.1.51
```
Linux-serverns nätverkskort kontrollerades med:

```bash
ip addr show
```
Nätverkskortet enp0s1 hade IPv4-adressen 192.168.1.50/24 och var aktivt.

 ### Windows – PowerShell

Mappen `C:\Systementor\KonsultData` skapades via PowerShell:

```powershell
New-Item -ItemType Directory -Path "C:\Systementor\KonsultData" -Force
```

Behörighetsstrukturen för mappen kontrollerades med:

```powershell
Get-Acl "C:\Systementor\KonsultData"
```

Nätverksanslutningen till Linux-VM verifierades med:

```powershell
Test-Connection 192.168.1.50
```

Windows nätverksinställningar kontrollerades med:

```powershell
ipconfig /all
```


## AI-logg och Utvärdering (Kursmål 11)

### Exakt prompt

> hur skapar jag en grupp i Linux, tilldelar mappen och filen till gruppen konsulter och ställer in behörigheter enligt principen om lägsta behörighet

### AI-verktygets svar

AI rekommenderade att skapa gruppen med:

```bash
sudo groupadd konsulter
```

Därefter rekommenderades att tilldela katalogen och filen gruppen `konsulter` med:

```bash
sudo chgrp konsulter /var/systementor/konsultdata
sudo chgrp konsulter /var/systementor/konsultdata/anteckningar.txt
```

För att ställa in behörigheterna rekommenderades:

```bash
sudo chmod 750 /var/systementor/konsultdata
sudo chmod 640 /var/systementor/konsultdata/anteckningar.txt
```

### Kritisk granskning och verifiering

Jag kontrollerade AI:s förslag genom att köra kommandona i min Linux-VM. Svaret var korrekt: gruppen `konsulter` skapades utan fel och kommandona gjorde det uppgiften krävde.

Jag hittade inga hallucinationer eller föråldrade kommandon. Behörigheterna 750 och 640 följer principen om lägsta behörighet, eftersom andra användare inte får någon åtkomst. Jag såg inga säkerhetsbrister i förslaget.

Jag verifierade resultatet genom att kontrollera katalogen och filen med:

```bash
ls -la /var/systementor/konsultdata
```

AI-svaret användes som stöd, men kommandona verifierades praktiskt innan resultatet dokumenterades.
