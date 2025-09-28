# Enumeration Workflow

## Myšlenkový proces

Enumeration není jen slepé spouštění nástrojů. Takhle bys měl přemýšlet:
- **Co vidím na první pohled?**
- **Co se autor snažil schovat?**
- **Jaké technologie používá?**
- **Kde mohou být skryté vstupy?**

## Master Checklist

### Fáze 1: První dojem (5-10 min)
- [ ] Jaký typ challenge to je? (web, network, app, mixed?)
- [ ] Máš IP nebo doménu?
- [ ] Základní ping test - odpovídá vůbec?
- [ ] Quick port scan - co běží?

### Fáze 2: Širší obzor (15-30 min)
- [ ] Kompletní port scan (CTFEnum nebo Legion pro GUI)
- [ ] Service detection na všech otevřených portech
- [ ] Základní banner grabbing
- [ ] Screenshot všeho, co má GUI

### Fáze 3: Deep dive na službách (hlavní čas)
- [ ] **Web services** → [Web Enumeration Workflow](./Web-Enumeration.md)
- [ ] **SSH/Telnet** → [Remote Access Enumeration](./Remote-Access.md)
- [ ] **File services** (FTP, SMB, NFS) → [File Services Enumeration](./File-Services.md)
- [ ] **Database services** → [Database Enumeration](./Database-Enumeration.md)
- [ ] **Email services** → [Email Services Enumeration](./Email-Services.md)

### Fáze 4: Information gathering (paralelně)
- [ ] **OSINT** → [OSINT Techniques](./OSINT.md)
- [ ] **Passive reconnaissance** → [Passive Recon](./Passive-Recon.md)
- [ ] **Certificate analysis**
- [ ] **Historical data mining**

## Mentální modely

### The "Expand and Contract" Pattern
1. **Expand** - najdi co nejvíc entry pointů
2. **Contract** - soustřeď se na nejslibnější
3. **Expand** - když se zasekneš, vrať se k širšímu pohledu

### The "Layer by Layer" Approach
- **Surface layer** - co je vidět hned
- **Configuration layer** - jak to je nastavené
- **Code layer** - jak to funguje uvnitř
- **Data layer** - co obsahuje

### The "Assume Nothing" Mindset
- Testuj i "zřejmé" věci
- Default credentials často fungují
- Každý port může skrývat překvapení
- Chybové hlášky jsou zlatý důl informací

## Pro Tips z praxe

### Časový management
- **První hodina** - široký přehled, nezabývej se detaily
- **Paralelizace** - nech běžet dlouhé scany na pozadí
- **Dokumentace** - zapisuj si všechno průběžně, ne až na konci

### Nástroje a workflow
- **CTFEnum** pro rychlý přehled portů
- **Legion** když chceš GUI a vizuální přehled
- **Burp Suite** vždy běžící pro web traffic
- **Terminal session** pro každou službu zvlášť

### Když se zasekneš
1. Vrať se k checklistu - co jsi přeskočil?
2. Změň perspektivu - zkus jiný nástroj
3. Hledej netypické porty a services
4. Zkontroluj, jestli neběží něco na UDP

## Organizace výsledků

### Struktura adresářů pro CTF
```
ctf-challenge/
├── recon/           # Všechny enumeration výsledky
├── web/            # Web specific findings
├── credentials/    # Nalezené hesla, hashe
├── exploits/       # Pracovní skripty
└── notes.md        # Průběžné poznámky
```

### Co si zapisovat
- **Každý spuštěný příkaz** s timestampem
- **Zajímavé chybové hlášky** - často obsahují hints
- **Nalezené credentials** - i když nepoužitelné hned
- **Hypotézy** - co si myslíš, že může fungovat

S tímto zápisem ti AI může pomoct o dost lépe.

## Vazby na další dokumenty

- [Web Enumeration Workflow](./Web-Enumeration.md) - Kompletní postup pro webové aplikace
- [Remote Access Enumeration](./Remote-Access.md) - SSH, Telnet, RDP
- [File Services Enumeration](./File-Services.md) - FTP, SMB, NFS
- [Database Enumeration](./Database-Enumeration.md) - MySQL, PostgreSQL, MSSQL
- [OSINT Techniques](./OSINT.md) - Pasivní sběr informací
- [Passive Recon](./Passive-Recon.md) - Certificate transparency, DNS, atd.

## Mindset shifty

### Když najdeš credential
Netestuj ho jen tam, kde jsi ho našel. Zkus ho všude - SSH, web admin, database, FTP...

### Když vidíš verzi software
Hned googluj CVE, ale nezapomeň na konfigurační chyby - často jsou jednodušší cesta.

### Když se něco tváří jako rabbit hole
Možná to rabbit hole není. Někdy je cesta dlouhá záměrně.
