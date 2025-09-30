# Enumeration Tools

## Port Scanning & Network Discovery

### nmap
**Použití**: Univerzální port scanner, service detection, OS fingerprinting
**Typ**: Command line
**Nejběžnější commands**:
```bash
# Základní TCP scan
nmap -sS -T4 target.com

# Service version detection
nmap -sV -sC target.com

# Kompletní TCP scan všech portů
nmap -sS -T4 -p- target.com

# UDP scan top portů
nmap -sU --top-ports 1000 target.com

# Aggressive scan s OS detection
nmap -A target.com

# Specific port range
nmap -p 80,443,22,21 target.com

# Output to file
nmap -oA scan_results target.com
```

### CTFEnum
**Použití**: Rychlý port scanning optimalizovaný pro CTF
**Typ**: Python tool
**Nejběžnější commands**:
```bash
# Základní scan
ctfenum target.com

# Scan s custom wordlisty
ctfenum -w wordlist.txt target.com

# Verbose output
ctfenum -v target.com

# Output to file
ctfenum -o results.txt target.com
```

### Legion
**Použití**: GUI nástroj pro automated enumeration
**Typ**: GUI application
**Využití**:
- Automatické stage-based enumeration
- Visual mapping síťové infrastruktury
- Integrated tool launching
- Real-time results display

### masscan
**Použití**: Rychlý port scanner pro velké rozsahy
**Typ**: Command line
**Nejběžnější commands**:
```bash
# Rychlý scan velkého rozsahu
masscan -p1-65535 192.168.1.0/24 --rate=1000

# Specific ports scan
masscan -p80,443 10.0.0.0/8 --rate=10000

# Output formáty
masscan -p80 target.com --rate=1000 -oX output.xml
```

### rustscan
**Použití**: Moderní rychlý port scanner
**Typ**: Command line (Rust)
**Nejběžnější commands**:
```bash
# Základní rychlý scan
rustscan -a target.com

# Custom port range
rustscan -a target.com -r 1-1000

# Piping to nmap
rustscan -a target.com -- -sV -sC
```

## Web Enumeration

### gobuster
**Použití**: Directory/file bruteforcing, subdomain discovery
**Typ**: Command line (Go)
**Nejběžnější commands**:
```bash
# Directory enumeration
gobuster dir -u http://target.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

# File enumeration s extensions
gobuster dir -u http://target.com -w wordlist.txt -x php,html,txt,js

# Subdomain enumeration
gobuster vhost -u http://target.com -w subdomains.txt

# DNS subdomain enumeration
gobuster dns -d target.com -w subdomains.txt

# Custom headers
gobuster dir -u http://target.com -w wordlist.txt -H "Authorization: Bearer token"
```

### ffuf
**Použití**: Fast web fuzzer pro directories, files, parameters
**Typ**: Command line (Go)
**Nejběžnější commands**:
```bash
# Directory fuzzing
ffuf -w wordlist.txt -u http://target.com/FUZZ

# File extension fuzzing
ffuf -w wordlist.txt -u http://target.com/FUZZ.php

# Parameter fuzzing
ffuf -w wordlist.txt -u http://target.com/page?FUZZ=value

# POST data fuzzing
ffuf -w wordlist.txt -u http://target.com/login -d "username=admin&password=FUZZ"

# Custom filters
ffuf -w wordlist.txt -u http://target.com/FUZZ -fc 404 -fs 1234
```

### dirsearch
**Použití**: Web path scanner s built-in wordlisty
**Typ**: Python
**Nejběžnější commands**:
```bash
# Základní directory scan
dirsearch -u http://target.com

# Custom wordlist
dirsearch -u http://target.com -w custom_wordlist.txt

# Multiple extensions
dirsearch -u http://target.com -e php,html,js,txt

# Recursive scanning
dirsearch -u http://target.com -r

# Custom threads a delay
dirsearch -u http://target.com -t 50 --delay=1
```

### whatweb
**Použití**: Web technology identification
**Typ**: Command line (Ruby)
**Nejběžnější commands**:
```bash
# Základní fingerprinting
whatweb target.com

# Aggressive mode
whatweb -a 3 target.com

# Output formáty
whatweb target.com --log-xml=results.xml

# Multiple targets
whatweb -i targets.txt
```

### nikto
**Použití**: Web vulnerability scanner
**Typ**: Command line (Perl)
**Nejběžnější commands**:
```bash
# Základní scan
nikto -h http://target.com

# Custom port
nikto -h target.com -p 8080

# SSL scan
nikto -h https://target.com -ssl

# Output to file
nikto -h target.com -o results.txt
```

### Burp Suite
**Použití**: Web application security testing platform
**Typ**: GUI (Java)
**Klíčové funkce**:
- Proxy pro HTTP/HTTPS traffic
- Spider pro automated crawling
- Scanner pro vulnerability detection
- Intruder pro automated attacks
- Repeater pro manual testing

## Subdomain & DNS Enumeration

### subfinder
**Použití**: Pasivní subdomain discovery
**Typ**: Command line (Go)
**Nejběžnější commands**:
```bash
# Základní subdomain enumeration
subfinder -d target.com

# Output to file
subfinder -d target.com -o subdomains.txt

# Silent mode
subfinder -d target.com -silent

# All sources
subfinder -d target.com -all
```

### amass
**Použití**: Comprehensive asset discovery
**Typ**: Command line (Go)
**Nejběžnější commands**:
```bash
# Passive enumeration
amass enum -passive -d target.com

# Active enumeration
amass enum -active -d target.com

# Brute force
amass enum -brute -d target.com

# Output formats
amass enum -d target.com -json results.json
```

### dnsrecon
**Použití**: DNS enumeration tool
**Typ**: Python
**Nejběžnější commands**:
```bash
# Standard enumeration
dnsrecon -d target.com

# Zone transfer attempt
dnsrecon -d target.com -a

# Brute force
dnsrecon -d target.com -D subdomains.txt -t brt

# Reverse lookup
dnsrecon -r 192.168.1.0/24
```

## Remote Access Enumeration

### ssh-audit
**Použití**: SSH configuration auditing
**Typ**: Python
**Nejběžnější commands**:
```bash
# SSH audit
ssh-audit target.com

# Specific port
ssh-audit target.com:2222

# JSON output
ssh-audit --json target.com
```

### enum4linux
**Použití**: SMB/NetBIOS enumeration
**Typ**: Perl script
**Nejběžnější commands**:
```bash
# Kompletní enumeration
enum4linux target.com

# Pouze shares
enum4linux -S target.com

# Users enumeration
enum4linux -U target.com

# Verbose output
enum4linux -v target.com
```

### smbclient
**Použití**: SMB client pro share access
**Typ**: Command line
**Nejběžnější commands**:
```bash
# List shares
smbclient -L //target.com -N

# Connect to share
smbclient //target.com/share -N

# Anonymous login
smbclient //target.com/share -U ""

# Authenticated access
smbclient //target.com/share -U username
```

### rpcclient
**Použití**: RPC client pro Windows enumeration
**Typ**: Command line
**Nejběžnější commands**:
```bash
# Null session
rpcclient -U "" -N target.com

# User enumeration
rpcclient> enumdomusers

# Share enumeration
rpcclient> netshareenum

# Domain info
rpcclient> querydominfo
```

## File Service Enumeration

### ftp
**Použití**: FTP client
**Typ**: Built-in command
**Nejběžnější commands**:
```bash
# Anonymous login
ftp target.com
# Username: anonymous
# Password: anonymous

# List files
ftp> ls -la

# Download file
ftp> get filename

# Upload file
ftp> put filename

# Binary mode
ftp> binary
```

### showmount
**Použití**: NFS exports enumeration
**Typ**: Command line
**Nejběžnější commands**:
```bash
# Show exports
showmount -e target.com

# Show all mount info
showmount -a target.com

# Show directories
showmount -d target.com
```

### rpcinfo
**Použití**: RPC service enumeration
**Typ**: Command line
**Nejběžnější commands**:
```bash
# List RPC services
rpcinfo -p target.com

# TCP services
rpcinfo -T tcp target.com

# Specific program
rpcinfo -u target.com nfs
```

## Database Enumeration

### mysql
**Použití**: MySQL client
**Typ**: Command line
**Nejběžnější commands**:
```bash
# Basic connection
mysql -h target.com -u root -p

# No password
mysql -h target.com -u root

# Execute command
mysql -h target.com -u root -e "SHOW DATABASES;"

# Script execution
mysql -h target.com -u root < script.sql
```

### psql
**Použití**: PostgreSQL client
**Typ**: Command line
**Nejběžnější commands**:
```bash
# Basic connection
psql -h target.com -U postgres

# Database selection
psql -h target.com -U postgres -d database_name

# Execute command
psql -h target.com -U postgres -c "SELECT version();"

# List databases
psql -h target.com -U postgres -l
```

### sqlcmd
**Použití**: MSSQL client
**Typ**: Command line
**Nejběžnější commands**:
```bash
# Basic connection
sqlcmd -S target.com -U sa -P password

# Windows authentication
sqlcmd -S target.com -E

# Execute query
sqlcmd -S target.com -U sa -Q "SELECT @@VERSION"

# Input file
sqlcmd -S target.com -U sa -i script.sql
```

### mongo
**Použití**: MongoDB client
**Typ**: Command line
**Nejběžnější commands**:
```bash
# Basic connection
mongo target.com:27017

# Database selection
mongo target.com:27017/database_name

# Authentication
mongo target.com:27017 -u username -p password

# Execute script
mongo target.com:27017 script.js
```

### redis-cli
**Použití**: Redis client
**Typ**: Command line
**Nejběžnější commands**:
```bash
# Basic connection
redis-cli -h target.com

# Custom port
redis-cli -h target.com -p 6380

# Authentication
redis-cli -h target.com -a password

# Execute command
redis-cli -h target.com KEYS "*"
```

## OSINT & Information Gathering

### theHarvester
**Použití**: Email/subdomain/IP harvesting
**Typ**: Python
**Nejběžnější commands**:
```bash
# Email harvesting
theHarvester -d target.com -b google

# Multiple sources
theHarvester -d target.com -b google,bing,linkedin

# Subdomain focus
theHarvester -d target.com -b sublist3r

# Output to file
theHarvester -d target.com -b all -f results.xml
```

### shodan
**Použití**: Internet-connected device search
**Typ**: Command line/Web
**Nejběžnější commands**:
```bash
# Search by hostname
shodan search hostname:target.com

# Search by organization
shodan search org:"Company Name"

# Search by port
shodan search port:22 country:US

# Download results
shodan download results.json.gz hostname:target.com
```

### recon-ng
**Použití**: Reconnaissance framework
**Typ**: Framework (Python)
**Základní workflow**:
```bash
# Start framework
recon-ng

# Install modules
marketplace install all

# Load workspace
workspaces create target_recon

# Add domain
add domains target.com

# Run modules
modules load recon/domains-hosts/google_site_web
run
```

## Multi-Purpose Tools

### nmap scripts
**Použití**: Specialized enumeration scripts
**Typ**: NSE scripts
**Nejběžnější scripts**:
```bash
# HTTP enumeration
nmap --script http-enum target.com

# SMB enumeration
nmap --script smb-enum-shares target.com

# Database enumeration
nmap --script mysql-enum target.com
nmap --script pgsql-brute target.com

# DNS enumeration
nmap --script dns-brute target.com

# SSL enumeration
nmap --script ssl-enum-ciphers target.com
```

### curl
**Použití**: HTTP client pro testing
**Typ**: Command line
**Nejběžnější commands**:
```bash
# Basic GET request
curl http://target.com

# Headers only
curl -I http://target.com

# POST request
curl -X POST -d "data=value" http://target.com

# Custom headers
curl -H "Authorization: Bearer token" http://target.com

# Follow redirects
curl -L http://target.com

# Save output
curl -o output.html http://target.com
```

### nc (netcat)
**Použití**: Network utility pro banner grabbing
**Typ**: Command line
**Nejběžnější commands**:
```bash
# Banner grabbing
nc target.com 80

# UDP connection
nc -u target.com 53

# Listen mode
nc -l -p 4444

# File transfer
nc target.com 1234 < file.txt

# Port scanning
nc -zv target.com 1-100
```

## Tool Selection Strategy

### Pro rychlý CTF overview:
1. **nmap** nebo **CTFEnum** pro port discovery
2. **gobuster** pro web enumeration
3. **whatweb** pro technology stack
4. **smbclient** pro SMB shares

### Pro kompletní enumeration:
1. **Legion** pro GUI-based automated scanning
2. **amass** pro comprehensive asset discovery
3. **Burp Suite** pro web application testing
4. **recon-ng** pro OSINT framework

### Pro stealth operations:
1. **masscan** s rate limiting
2. **subfinder** pro passive discovery
3. **shodan** pro passive intelligence
4. **theHarvester** pro OSINT

---