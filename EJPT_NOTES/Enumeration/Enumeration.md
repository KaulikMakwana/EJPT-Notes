# Host Enumeration

``` shell
1) "Host Discovery"
	# using fping 
	- fping -a -g 192.168.1.1/24 2>/dev/null
	# using nmap 
	- nmap -sn 192.168.1.1/24
	# host discovery and short livehosts 
	- nmap -sn 192.168.0.5/24 | grep -E 'Nmap scan report for|Service Info' | tee -a livehost.txt

2) "Port Scanning"
	# use tools like nmap masscan ,other which you like
	- nmap --min-rate 10000 -p- -T4 -oA portscan 192.168.1.1

3) "Aggressive scan"
	- sudo nmap -sSV -sC -A -v -T4 -oA Scan 192.168.1.1

```

---
# Service Enumeration

``` shell
1) "using nmap/metasploit"
	`use nmap script / metasploit modules`

2) "for smb shares"
	# use tools 
	- smbclient, smbmap, enum4linux, impacket-smbclient, others
	# manual (if you managed to have shares wordlists )
	- for i in $(cat shares.txt );do smbclient //target.ine.local/$i -N -c "ls" 2>/dev/null;done

3) "For service or web Enumeration"
	`use nmap script ; metasploit modules
	nessus ; burpsuite; zap, nuclei 
	or any other suitable automated tools`

	# like for snmp -> snmpwalk ; onesixtyone
	# for netbios -> nbtscan ; nbtlookup 
	# for ldap -> ldapsearch
	-> 'there are for each service specilised tool available ! use it '
	
```

---
# Usefull Wordlists
``` shell
1) "/usr/share/metasploit-framework/data/wordlists/"
	# usernames
	- common_users.txt
	- common_root.txt
	
	# passwords
	- unix_passwords.txt 'OR' user 
	- common_passwords.txt
	
	# other
	- sensitive_files.txt
	
2) "rockyou.txt"
	# use popular rockyou.txt if other not work 
	- /usr/share/wordlists/rockyou.txt # [unzip] gzip -d rockyou.txt.gz

3) "commix's default wordlists"
	"path" : /usr/share/commix/src/txt/
	- usernames.txt  'OR' default_usernames.txt
	- passwords.txt  'OR' default_passwords.txt

4) "webshells"
	# asp , php, and other webshells
	- ls /usr/share/webshells/asp/
	-> cmdasp.asp (windows - iis)

	# php webshell 
	- ls /usr/share/webshells/php/php-reverse-shell.php - linux (apache)

5) "windows executables"
	- /usr/share/windows-resources/binaries/

```

---
# Vulnerability / exploit / cve research 
``` shell
1) "tools"
	- searchsploit              # Local Exploit-DB search
	- getsploit                 # Fetch exploits from GitHub (CLI)
	- sploitscan                # CLI scanner for known CVEs

2) "online services"
	- exploitdb                # https://www.exploit-db.com/
	- packetstorm              # https://packetstormsecurity.com/
	- mitre                    # https://cve.mitre.org/
	- nvd                      # https://nvd.nist.gov/
	- vulners                  # https://vulners.com/
	- cvedetails               # https://cvedetails.com/
	- vulmon                   # https://vulmon.com/
	- rapid7                   # https://www.rapid7.com/db/
	- github                   # https://github.com/search?q=CVE
	- cxsecurity               # https://cxsecurity.com/
	- shodan                   # https://www.shodan.io/
	- censys                   # https://censys.io/
	- vulncheck                # https://vulncheck.com/
	- dnsdumpster              # https://dnsdumpster.com/
```

# mysql commands

``` shell 

- use wordpress;
- show tables;
- select * from wp_users;
- UPDATE wp_users SET user_pass = MD5('password123') WHERE user_login = 'admin';

```