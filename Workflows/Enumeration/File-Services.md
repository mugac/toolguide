# File Services Enumeration

## Overview
File services (FTP, SMB, NFS) jsou goldmine pro CTF. Často obsahují credentials, backup soubory, source code, nebo umožňují upload malicious files.

## Service Types & Approach

### FTP (Port 21, 990)
- **Anonymous access** - nejčastější quick win
- **Directory traversal** - ../../../ tricks
- **Upload capabilities** - web shell
- **Passive vs Active** - connection modes

### SMB/CIFS (Ports 139, 445)
- **Null sessions** - unauthenticated access  
- **Share enumeration** - co je dostupné?
- **Version detection** - SMBv1, v2, v3 differences
- **User enumeration** - RID cycling, LSA queries

### NFS (Port 2049)
- **Export enumeration** - showmount queries
- **Mount permissions** - who can mount what?
- **File permissions** - UID/GID mapping issues
- **Root squashing** - security mechanism bypass

## Enumeration Methodology

### FTP Deep Dive
```
1. Anonymous login test
2. Banner analysis 
3. Directory listing
4. File download/upload tests
5. Hidden file discovery
6. Binary vs ASCII mode testing
```

### SMB Deep Dive  
```
1. SMB version detection
2. Null session testing
3. Share enumeration
4. User/group enumeration  
5. Password policy detection
6. File permission analysis
```

### NFS Deep Dive
```
1. RPC service detection
2. Export list enumeration
3. Mount permission testing
4. File system analysis
5. Permission bypass attempts
6. Root squashing testing
```

## Systematic Checklist

### FTP Enumeration
- [ ] Anonymous login (anonymous:anonymous, ftp:ftp)
- [ ] Banner grabbing pro version info
- [ ] Directory listing (LIST, NLST commands)
- [ ] Hidden files (ls -la behavior)
- [ ] Upload testing (STOR command)
- [ ] Download all accessible files
- [ ] Binary/ASCII mode differences
- [ ] Passive/Active mode testing

### SMB Enumeration
- [ ] SMB version detection
- [ ] Null session testing (smbclient -N)
- [ ] Share listing (smbclient -L)
- [ ] Each share enumeration
- [ ] User enumeration (enum4linux)
- [ ] Group enumeration
- [ ] Password policy detection
- [ ] File download/upload testing

### NFS Enumeration
- [ ] RPC services scan (rpcinfo)
- [ ] Export enumeration (showmount -e)
- [ ] Mount attempt na každý export
- [ ] File permission analysis
- [ ] UID/GID mapping testing
- [ ] Root squashing bypass attempts
- [ ] Hidden file discovery

## Attack Vectors by Service

### FTP Attack Patterns
1. **Anonymous access** → file system browsing
2. **Weak credentials** → admin:admin, test:test
3. **Directory traversal** → ../../../etc/passwd
4. **Upload attacks** → web shell placement
5. **Binary inspection** → reverse engineering

### SMB Attack Patterns
1. **Null sessions** → user/share enumeration
2. **Weak passwords** → admin:password
3. **Share mounting** → file system access
4. **Credential harvesting** → password files
5. **Lateral movement** → other systems access

### NFS Attack Patterns
1. **Export mounting** → direct file access
2. **Permission bypass** → UID spoofing
3. **Root squashing bypass** → privilege escalation
4. **File modification** → backdoor placement
5. **Information disclosure** → sensitive file access

## Contextual Thinking

### Co hledat v souborech:
- **Config files** - database credentials, API keys
- **Backup files** - source code, SQL dumps
- **Log files** - usernames, IP addresses, patterns
- **User files** - SSH keys, password files
- **Application files** - source code analysis

### Naming patterns které často fungují:
- **Backup extensions**: .bak, .backup, .old, .orig
- **Config files**: config.*, .env, settings.*
- **Database files**: *.sql, *.db, *.sqlite
- **Keys**: *.key, *.pem, id_rsa, authorized_keys
- **Temp files**: *.tmp, *.temp, ~*

## Tool Integration

*Specific commands v [Tools/Enumeration](../../Tools/Enumeration/)*

### Multi-service tools:
- **enum4linux** - SMB + some NFS
- **smbclient** - SMB interaction
- **showmount** - NFS exports
- **rpcinfo** - RPC services

### File analysis tools:
- **file** - file type detection
- **strings** - readable strings extraction
- **hexdump** - binary analysis
- **grep** - content searching

## Documentation Strategy

### Pro každý service zaznamenej:
```
Service: FTP
Port: 21
Access: Anonymous allowed
Interesting files: 
- /backup/database.sql
- /web/config.php
- /users/john/.ssh/id_rsa
Permissions: Read/Write on /upload/
Next steps: Upload web shell to /upload/
```

## Advanced Techniques

### FTP Advanced
- **Bounce attacks** - použití FTP pro port scanning
- **Data channel manipulation** - active/passive exploitation
- **Protocol fuzzing** - unexpected command responses

### SMB Advanced  
- **SMB relay attacks** - credential forwarding
- **DCE/RPC exploitation** - specific RPC calls
- **Pipeline attacks** - command chaining

### NFS Advanced
- **UID manipulation** - fake user creation
- **Mount option exploitation** - rw,no_root_squash
- **Portmapper exploitation** - RPC service abuse

## Integration Points

### Connection s dalšími findings:
- **Credentials z web** → FTP/SMB authentication
- **Usernames z OSINT** → targeted attacks
- **IP ranges z network scan** → lateral movement
- **File paths z web enum** → targeted file searches
