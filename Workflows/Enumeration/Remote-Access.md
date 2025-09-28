# Remote Access Enumeration

## Cíl
Identifikovat a otestovat všechny remote access služby. SSH, Telnet, RDP - každá má svoje specifika a často obsahuje nejrychlejší cestu k systému.

## Services Overview

### SSH (Port 22)
- **Co hledat**: Banner info, authentication methods, weak credentials
- **Red flags**: Old versions, password auth enabled, weak configs

### Telnet (Port 23)
- **Co hledat**: Unencrypted protocols, default credentials, system info
- **Red flags**: Telnet v 2025 = většinou easy target

### RDP (Port 3389)
- **Co hledat**: Windows hosts, credential attacks, session hijacking
- **Red flags**: Weak passwords, no network level auth

### VNC (Porty 5900+)
- **Co hledat**: Remote desktop access, weak/no passwords
- **Red flags**: Default passwords, unencrypted connections

## Enumeration Checklist

### SSH Enumeration
- [ ] Banner grabbing - verze a OS info
- [ ] Authentication methods (pubkey, password, keyboard-interactive)
- [ ] User enumeration (pokud možné)
- [ ] Host key fingerprinting
- [ ] Algorithm negotiation testing

### Telnet Enumeration  
- [ ] Banner grabbing
- [ ] Service identification (často ne telnet, ale jiná služba)
- [ ] Authentication prompts
- [ ] System information leakage
- [ ] Command injection testing

### RDP Enumeration
- [ ] Service detection a verze
- [ ] NLA (Network Level Authentication) status
- [ ] Certificate information
- [ ] User enumeration možnosti
- [ ] Credential testing safe methods

### VNC Enumeration
- [ ] Service detection na všech VNC portech
- [ ] Authentication method detection
- [ ] Version fingerprinting
- [ ] Screen capture attempts (if possible)

## Tactical Thinking

### SSH Strategy
1. **Version analysis** - starší verze = možné CVEs
2. **Config weaknesses** - root login, password auth
3. **Credential attacks** - ale opatrně kvůli rate limiting
4. **Key-based auth** - hledej private keys jinde

### Telnet Strategy
1. **Protocol confusion** - často to není telnet
2. **Raw socket communication** - pošli různé inputs
3. **Service fingerprinting** - co skutečně běží?
4. **Credential testing** - obvykle slabé hesla

### RDP Strategy
1. **Version detection** - Windows version hints
2. **Certificate analysis** - domain information
3. **Session enumeration** - active sessions?
4. **Brute force consideration** - account lockout policies

## Systematic Approach

### Phase 1: Discovery & Fingerprinting
```bash
# Quick service detection
nmap -sV -p 22,23,3389 target

# Banner grabbing
nc target 22
nc target 23
nc target 3389
```

### Phase 2: Service-Specific Enumeration
*Detailní commands v [Tools/Enumeration](../../Tools/Enumeration/)*

### Phase 3: Authentication Testing
- [ ] Common credentials testing
- [ ] Service-specific default accounts
- [ ] Credential stuffing (pokud máš wordlisty)
- [ ] Certificate/key analysis

### Phase 4: Exploitation Preparation
- [ ] Version-specific exploits research
- [ ] Configuration weakness documentation
- [ ] Valid credentials organization
- [ ] Next steps planning

## Context-Specific Approaches

### CTF vs Real World
**CTF context**:
- Default credentials častěji fungují
- Rate limiting často není implementováno
- Focus na rychlé wins než stealth

**Production mindset**:
- Buď opatrný s brute force
- Dokumentuj vše pro reporting
- Hledej config chyby

### Integration s ostatními findings
- **Web app credentials** → zkus na SSH
- **Config files** → hledej SSH keys, RDP certs
- **User enumeration** → targeted credential attacks
- **Version info** → CVE research

## Tool Categories

*Specific tools a usage v Tools sekci*

### Information Gathering
- Banner grabbing tools
- Version detection utilities
- Service fingerprinting tools

### Authentication Testing
- Credential testers
- Brute force tools (opatrně!)
- Key/certificate analyzers

### Exploitation Tools
- Service-specific exploits
- Session hijacking tools
- Privilege escalation helpers

## Pro Tips

### SSH Specific
- Sleduj `.ssh/` directories v jiných services
- Private keys často v web directories, backups
- `authorized_keys` soubory mohou odhalit valid usernames
- SSH tunneling možnosti pro pivot

### Telnet Specific
- Často to není telnet - zkus HTTP, custom protocols
- Raw TCP communication často odhalí víc
- Historicky slabé service - hledej easy wins

### RDP Specific
- BlueKeep a podobné RCE vulnerabilities
- Pass-the-hash attacks
- Session token stealing
- Network level authentication bypass

### VNC Specific
- Default passwords: admin, password, blank
- Více instancí na různých portech
- Screen viewing bez authentication
- Reverse VNC connections

## Security Considerations

### Rate Limiting
- SSH často má fail2ban nebo podobné
- RDP má account lockout policies
- Telnet obvykle neomezený
- VNC většinou bez rate limiting

### Logging & Detection
- SSH přístupy jsou silně monitorované
- RDP events jsou Windows logged
- Telnet connections obvykle minimálně logged
- VNC se často neloguje

### Noise Level
- **Tichý**: Banner grabbing, version detection
- **Střední**: Single credential tests
- **Hlučný**: Brute force, rapid connections
