# OSINT Techniques pro CTF

## OSINT Mindset
V CTF není OSINT jen o tom najít informace o targetu - často jsou tam schované hints, credentials nebo vektory útoku. Tvůrci často používají real-world data jako inspiraci.

## Information Gathering Layers

### Public Records & Registrations
- **Domain registrations** - whois data, contact info
- **SSL certificates** - certificate transparency logs
- **DNS records** - subdomains, mail servers, SPF records
- **IP allocations** - network ranges, geolocation

### Social & Professional Networks
- **LinkedIn** - employee info, technologies used
- **GitHub** - repositories, code leaks, commit history
- **Stack Overflow** - technical questions, usernames
- **Company websites** - technologies, contact info

### Search Engine Intelligence
- **Google dorking** - site-specific searches
- **Bing/DuckDuckGo** - different indexing
- **Shodan** - internet-connected devices
- **Censys** - internet-wide scanning data

### Historical & Archived Data
- **Wayback Machine** - website history
- **Archive.today** - recent snapshots
- **Google Cache** - cached versions
- **Git history** - deleted commits, old versions

## Systematic OSINT Checklist

### Domain Intelligence
- [ ] Whois information analysis
- [ ] DNS record enumeration (A, AAAA, MX, TXT, NS)
- [ ] Subdomain discovery (passive methods)
- [ ] Certificate transparency log search
- [ ] Historical DNS data

### Search Engine Reconnaissance
- [ ] Google dorking s target domain
- [ ] File type searches (filetype:pdf, filetype:doc)
- [ ] Site structure mapping (site:target.com)
- [ ] Error page discovery (site:target.com "error")
- [ ] Login page discovery (site:target.com "login")

### Social Engineering Intelligence
- [ ] Employee enumeration (LinkedIn, company website)
- [ ] Email pattern identification
- [ ] Social media presence analysis
- [ ] Professional network mapping
- [ ] Company structure research

### Technical Infrastructure
- [ ] Shodan device enumeration
- [ ] Censys certificate analysis
- [ ] IP range identification
- [ ] Technology stack identification
- [ ] Third-party service integration

## OSINT Strategy Framework

### The "Ripple Effect" Method
1. **Start broad** - basic domain/company info
2. **Follow connections** - related domains, employees
3. **Deep dive specifics** - interesting findings
4. **Cross-reference** - verify across multiple sources
5. **Document everything** - build comprehensive picture

### The "Timeline Approach"
- **Current state** - what's visible now
- **Recent changes** - last 6 months activity
- **Historical view** - years of data
- **Pattern identification** - recurring themes, names

### The "Multi-Source Verification"
- Never trust single source
- Cross-check information
- Look for inconsistencies
- Verify through different methods

## OSINT Toolchain

*Specific tools a techniques v [Tools/Enumeration](../../Tools/Enumeration/)*

### Passive DNS/Domain Tools
- **whois** - domain registration info
- **dig/nslookup** - DNS queries
- **subfinder** - subdomain discovery
- **amass** - comprehensive asset discovery

### Search & Discovery
- **Google** - advanced search operators
- **Shodan** - internet device search
- **Censys** - certificate/device intelligence
- **theHarvester** - email/subdomain harvesting

### Historical & Archive Tools
- **Wayback Machine** - web.archive.org
- **archive.today** - recent snapshots
- **Google Cache** - cache:target.com
- **GitHub search** - code repositories

## Google Dorking Cookbook

### Site-Specific Searches
```
site:target.com filetype:pdf
site:target.com inurl:admin
site:target.com intext:"password"
site:target.com "confidential" OR "internal"
site:target.com ext:sql OR ext:db
```

### Technology Discovery
```
site:target.com "powered by"
site:target.com "version" OR "v1." OR "v2."
site:target.com inurl:api OR inurl:v1 OR inurl:v2
site:target.com "swagger" OR "api documentation"
```

### Credential & Config Hunting
```
site:target.com "database" AND ("password" OR "username")
site:target.com filetype:env OR filetype:config
site:target.com "api_key" OR "secret_key"
site:target.com ".git" OR "git clone"
```

### Error & Debug Info
```
site:target.com "fatal error" OR "warning" OR "error"
site:target.com "debug" OR "test" OR "staging"
site:target.com "stack trace" OR "exception"
```

## CTF-Specific OSINT Patterns

### Challenge Metadata
- **Challenge descriptions** - hidden hints, wordplay
- **Author information** - their other work, patterns
- **File metadata** - creation dates, software used
- **Image analysis** - EXIF data, steganography hints

### Historical Context
- **Previous CTFs** - similar challenges, reused assets
- **Team/organizer patterns** - preferred techniques
- **Technology preferences** - commonly used stacks
- **Naming conventions** - pattern recognition

### Cross-Reference Opportunities
- **Usernames across platforms** - same handles everywhere
- **Email patterns** - corporate vs personal
- **Infrastructure reuse** - same servers/domains
- **Code similarities** - GitHub repositories

## Advanced OSINT Techniques

### Certificate Transparency Mining
```bash
# Search pro všechny certs pro domain
curl -s "https://crt.sh/?q=%.target.com&output=json" | jq -r '.[].name_value' | sort -u

# Historical certificate data
curl -s "https://crt.sh/?id=CERT_ID&output=json"
```

### Wayback Machine API
```bash
# Get all snapshots
curl "http://web.archive.org/cdx/search/cdx?url=target.com/*&output=json"

# Specific timeframe
curl "http://web.archive.org/cdx/search/cdx?url=target.com&from=2020&to=2021&output=json"
```

### Shodan Advanced Queries
```
hostname:target.com
ssl.cert.subject.cn:target.com
org:"Company Name"
port:22 country:US city:"New York"
```

## Information Correlation Matrix

### Cross-Reference Framework
| Source | Domain Info | User Info | Tech Stack | Infrastructure |
|--------|-------------|-----------|------------|----------------|
| Whois | ✓ | ✓ | - | ✓ |
| Certs | ✓ | - | - | ✓ |
| GitHub | - | ✓ | ✓ | ✓ |
| LinkedIn | - | ✓ | ✓ | - |
| Shodan | ✓ | - | ✓ | ✓ |

### Confidence Levels
- **High confidence**: Multiple sources confirm
- **Medium confidence**: Single reliable source
- **Low confidence**: Unverified or outdated
- **Speculation**: Educated guess based on patterns

## OSINT Ethics & Legal

### CTF Context
- **Public information only** - nothing requiring unauthorized access
- **Respect rate limits** - don't hammer APIs/services
- **Document sources** - track where info came from
- **Verify accuracy** - outdated info can mislead

### Operational Security
- **Use VPN/Tor** - protect your identity
- **Rotate user agents** - avoid fingerprinting
- **Pace requests** - look human-like
- **Clean browser state** - prevent tracking
