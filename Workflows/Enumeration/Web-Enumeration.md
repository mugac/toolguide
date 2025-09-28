# Web Enumeration Workflow

## Mindset pro web enumeration
Web aplikace jsou často největší attack surface v CTF. Každá stránka, každý parametr, každá chybová hláška může být klíčem k řešení.

## Quick Start Checklist

### První minuty (5 min max)
- [ ] Otevři si aplikaci v browseru - co vidíš?
- [ ] Zkontroluj zdrojový kód (Ctrl+U)
- [ ] Otevři Developer Tools (F12)
- [ ] robots.txt, sitemap.xml
- [ ] Základní screenshot pro dokumentaci

### Discovery fáze (15-30 min)
- [ ] Directory/file bruteforcing
- [ ] Subdomain enumeration (pokud je to doména)
- [ ] Parameter discovery
- [ ] Technology stack identification
- [ ] Cookie analysis

### Crawling & Mapping (paralelně)
- [ ] Spider/crawl všechny nalezené stránky
- [ ] Manuální procházení funkcionalit
- [ ] Form analysis
- [ ] JavaScript file analysis

## Thinking Framework

### Co hledat v pořadí priority:
1. **Vstupní body** - formuláře, URL parametry, cookies
2. **Skryté content** - admin panely, backup soubory, .git
3. **Technology fingerprints** - versions, frameworks
4. **Konfigurační chyby** - debug módy, verbose errors
5. **Business logic flaws** - neočekávané workflows

### Layered approach:
- **Frontend layer** - co vidíš v browseru
- **HTTP layer** - headers, requests, responses
- **Backend layer** - server technologies, databases
- **File system layer** - directory structure, backup files

## Nástroje pro jednotlivé úkoly

*Detailní usage v [Tools/Enumeration](../../Tools/Enumeration/)*

### Directory/File Discovery
- **gobuster** - rychlý a spolehlivý
- **ffuf** - flexibilní fuzzing
- **dirsearch** - Python based s dobrými wordlisty
- **feroxbuster** - rekurzivní s rate limiting

### Subdomain Discovery
- **subfinder** - pasivní discovery
- **amass** - comprehensive OSINT
- **gobuster vhost** - brute force approach

### Technology Detection
- **whatweb** - command line fingerprinting
- **wappalyzer** - browser extension
- **builtwith** - online service

## Systematic Checklist

### Initial Reconnaissance
- [ ] Manual browse - click through everything
- [ ] View source - comments, hidden fields, JS variables
- [ ] robots.txt, sitemap.xml, security.txt
- [ ] HTTP headers analysis
- [ ] SSL certificate inspection (if HTTPS)

### Content Discovery
- [ ] Common directories (/admin, /backup, /test, /dev)
- [ ] Common files (config.php, .env, .git, .svn)
- [ ] Backup files (.bak, .old, .backup, ~)
- [ ] Technology-specific paths (WordPress, Drupal, etc.)

### Parameter Analysis
- [ ] GET parameters in URLs
- [ ] POST parameters in forms
- [ ] Hidden form inputs
- [ ] Cookie parameters
- [ ] HTTP headers jako vectors

### JavaScript Analysis
- [ ] Inline JavaScript pro credentials/endpoints
- [ ] External JS files
- [ ] AJAX endpoints
- [ ] API documentation in comments
- [ ] Source maps (.map files)

### Error Handling
- [ ] 404 error pages - custom vs default
- [ ] 403 forbidden - může odhalit existující paths
- [ ] 500 errors - stack traces s info
- [ ] SQL errors - database fingerprinting
- [ ] Application errors - technology hints

## Tactical Approaches

### The "Breadcrumb" Method
Sleduj všechny odkazy a references:
- URL structures → directory patterns
- CSS/JS includes → development paths
- Image sources → upload directories
- Comments → developer notes

### The "Version Hunt"
Každá verze = potenciální CVE:
- Framework versions v response headers
- Library versions v JavaScript
- Server software v banners
- CMS versions v meta tags

### The "Context Switch"
Přemýšlej z různých perspektiv:
- **User perspective** - co má dělat aplikace?
- **Developer perspective** - jak by to naprogramoval?
- **Admin perspective** - jaké má nástroje?
- **Attacker perspective** - kde jsou slabiny?

## Common Pitfalls

### Co nedělat
- Neřiď se jen automatickými nástroji
- Nepřehlížej "nudné" stránky
- Nezapomínej na edge cases (různé HTTP metody)
- Neřeš jen happy path - testuj error states

### Co dělat
- Kombinuj automatické + manuální testování
- Dokumentuj každý zajímavý endpoint
- Testuj různé input vectors
- Sleduj všechny HTTP responses

## Integration Points

### Kam dále po web enumeration:
- **Nalezené credentials** → zkus na SSH, databases
- **Upload functionality** → file upload attacks
- **Database hints** → [Database Enumeration](./Database-Enumeration.md)
- **API endpoints** → API security testing
- **Admin panels** → privilege escalation paths

## Pro Tips

### Time Management
- **5 min** - quick manual browse
- **15 min** - automated discovery tools
- **30 min** - deep dive na zajímavé findings
- **Rest** - manual testing & analysis

### Note Taking
Zapisuj si formátem:
```
URL: http://target.com/admin
Status: 403 Forbidden
Notes: Directory exists but access denied
Next: Try different HTTP methods, parameter fuzzing
```

### Wordlist Strategy
- Začni s malými wordlisty pro rychlý overview
- Pak použij větší pro kompletní coverage
- Custom wordlisty based na findings (cewl nástroj)
