# Labbdokumentation

**Namn:** Isman Bishaar Mahamud
**Datum:** 2026-09-28
**Kurs:** Introduktion till yrkesrollen och grunderna i IT-infrastruktur (MYH 2025/4008)

## 1. Introduktion

Denna dokumentation beskriver uppsättningen av en virtuell labbmiljö med en Linux-server och en Windows-klient. Dokumentationen innehåller även kommandoradsarbete i Linux och Windows, Git-versionering samt en kritisk reflektion kring användningen av generativ AI.

## 2. Git & versionshantering (Kursmål 10)

Projektet skapades i en lokal mapp på datorn och initierades som ett Git-repository via kommandoraden:

```bash
git init
```

Huvuddokumentationen skapades i Markdown-format som `Labbdokumentation.md`.

Arbetet sparades löpande med separata commits och tydliga commit-meddelanden. Commit-historiken verifierades med:

```bash
git log --oneline
```

Commit-historiken visar totalt 8 separata commits:

```bash
521290f Lägg till Git-avsnitt och utskrifter
2df1176 Rätta PowerShell-formatering
b447567 Rätta PowerShell-formatering
415d07d Slutför labbdokumentationen
34157c0 Förbättra Markdown-formatering
b03470f Dokumentera kommandoradsarbete
31c42b8 Dokumentera labbmiljö och nätverk
1bfd884 Skapa grundstruktur för labbdokumentation
```

Historiken visar att dokumentationen har utvecklats steg för steg under arbetsprocessen och uppfyller kravet på minst 4–5 separata commits.

**GitHub-repository:**
https://github.com/isman-bishaar/Labbmilj-Git-CLI-och-AI.git
## 3. Labbmiljö & nätverk (Kursmål 8)

Labbmiljön består av två virtuella maskiner som körs i UTM. Linux-servern och Windows-klienten är anslutna till samma Host-Only-nätverk, `Network 0`, så att de kan kommunicera med varandra.

| Hostname          | Operativsystem | IP-adress      | Subnätmask      | Standard Gateway |
| ----------------- | -------------- | -------------- | --------------- | ---------------- |
| `linux-server`    | Linux/Ubuntu   | `192.168.1.50` | `255.255.255.0` | Ingen            |
| `WIN-2PRLUV1NOVH` | Windows 11     | `192.168.1.51` | `255.255.255.0` | Ingen            |

Linux använder den statiska IP-adressen `192.168.1.50/24` och Windows använder `192.168.1.51/24`. Båda maskinerna ligger därför i samma subnät.

Kommunikationen testades i båda riktningarna. Från Linux användes:

```bash
ping 192.168.1.51
```

Testet gav svar från Windows-klienten och visade 0 % paketförlust.

Från Windows användes:

```powershell
Test-Connection 192.168.1.50
```

Även detta test lyckades.

## 4. Kommandoradsgenomförande (Kursmål 9)

### 4.1 Linux – Bash

Katalogen `/var/systementor/konsultdata` skapades via kommandoraden:

```bash
sudo mkdir -p /var/systementor/konsultdata
```

Filen `anteckningar.txt` skapades:

```bash
sudo touch /var/systementor/konsultdata/anteckningar.txt
```

Gruppen `konsulter` skapades:

```bash
sudo groupadd konsulter
```

Katalogen och filen tilldelades gruppen `konsulter`:

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

Nätverkskortet `enp0s1` hade IPv4-adressen `192.168.1.50/24` och var aktivt.

### 4.2 Windows – PowerShell

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

## 5. AI-logg & reflektion (Kursmål 11)

### 5.1 Exakt prompt

> hur skapar jag en grupp i Linux, tilldelar mappen och filen till gruppen konsulter och ställer in behörigheter enligt principen om lägsta behörighet

### 5.2 AI-verktygets svar

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

### 5.3 Kritisk granskning och verifiering

Jag kontrollerade AI:s förslag genom att köra kommandona i min Linux-VM. Gruppen `konsulter` skapades utan fel och kommandona gav det förväntade resultatet.

Jag verifierade även behörigheterna genom att kontrollera katalogen och filen med:

```bash
sudo ls -la /var/systementor/konsultdata
```

Resultatet visade att katalogen hade behörigheten `750` och filen hade `640`. Detta stämmer med den valda behörighetsmodellen.

Jag verifierade även nätverksinställningarna med `ip addr show` och nätverksanslutningen med `ping`.

AI-svaret användes som stöd, men kommandona verifierades praktiskt i den aktuella labbmiljön innan resultatet dokumenterades.
