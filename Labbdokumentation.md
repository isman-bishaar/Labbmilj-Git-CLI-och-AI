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

## 5. AI-logg och Utvärdering (Kursmål 11)

### 5.1 Exakt prompt

```text
hur skapar jag en grupp i Linux, tilldelar mappen och filen till gruppen konsulter och ställer in behörigheter enligt principen om lägsta behörighet
```

### 5.2 AI-verktygets svar

För att skapa en grupp i Linux och tilldela en mapp och en fil till gruppen `konsulter` kan du använda följande kommandon.

Först skapar du gruppen:

```bash
sudo groupadd konsulter
```

Skapa sedan katalogen och filen om de inte redan finns:

```bash
sudo mkdir -p /var/systementor/konsultdata
sudo touch /var/systementor/konsultdata/anteckningar.txt
```

Tilldela därefter katalogen och filen till gruppen `konsulter`:

```bash
sudo chgrp konsulter /var/systementor/konsultdata
sudo chgrp konsulter /var/systementor/konsultdata/anteckningar.txt
```

För att följa principen om lägsta behörighet kan du sätta följande behörigheter:

```bash
sudo chmod 750 /var/systementor/konsultdata
sudo chmod 640 /var/systementor/konsultdata/anteckningar.txt
```

Behörigheten `750` på katalogen innebär att ägaren har fullständiga rättigheter, gruppen kan läsa, skriva och gå in i katalogen, medan andra användare inte har någon åtkomst.

Behörigheten `640` på filen innebär att ägaren kan läsa och skriva, gruppen kan läsa och andra användare inte har någon åtkomst.

Kontrollera slutligen resultatet med:

```bash
sudo ls -la /var/systementor/konsultdata
```

På så sätt kan du verifiera att gruppen och behörigheterna har ställts in korrekt.

### 5.3 Kritisk granskning och verifiering

Jag granskade AI-verktygets svar och kontrollerade kommandona praktiskt i min Linux-VM.

Jag verifierade att gruppen `konsulter` skapades med:

```bash
sudo groupadd konsulter
```

Jag kontrollerade även att katalogen och filen kunde tilldelas gruppen `konsulter` med:

```bash
sudo chgrp konsulter /var/systementor/konsultdata
sudo chgrp konsulter /var/systementor/konsultdata/anteckningar.txt
```

Behörigheterna sattes med:

```bash
sudo chmod 750 /var/systementor/konsultdata
sudo chmod 640 /var/systementor/konsultdata/anteckningar.txt
```

Resultatet kontrollerades med:

```bash
sudo ls -la /var/systementor/konsultdata
```

Jag verifierade även nätverksinställningarna med `ip addr show` och nätverksanslutningen med `ping`.

Jag identifierade inga hallucinationer eller föråldrade kommandon i det AI-svar som användes. Jag kontrollerade även behörigheterna och såg att katalogen hade `750` och filen hade `640`. Det innebar att andra användare inte fick någon åtkomst, vilket stämde med den valda behörighetsmodellen.

AI-svaret användes som stöd, men jag verifierade kommandona praktiskt i den aktuella labbmiljön innan resultatet dokumenterades.

