# Enumerating System Information
1) **meterpreter commands :**
```shell
- getuid
- sysinfo
- show_mount # show mounts/drives of system is connected with
- hashdump   # get acounts hash dumps
```

2) **windows commands :**
``` powershell
$ hostname
$ systeminfo # you can grab hotfix ids

# for hotfix ids (alternative command)
$ wmic qfe get Caption,Description,HotFixID,InstalledOn 
```

---
# Enumerating Users & Groups
1) **meterpreter commands :**
```shell
$ getprivs # what kind of privileges have for current user
 
# logged on users enum | msfconsole
$ search logged_on >> post module : enum_logged_on_users 
```

2) **windows commands :**
```powershell
## user enum
$ whoami       # current user
$ whoami /priv # current user privileges
$ query user   # current logged on  user
$ net users    # all other users available in system
$ net user administrator ## info enum for admin

## group enum
$ net localgroup     # all local groups available in system
$ net localgroup administrators # members that are parts of administrator
$ net localgroup "Remote Desktop Users"
```

---
# Enumerating Network Information

1) **meterpreter commands:**
```shell
- ipconfig
- route
```
2) **windows commands :**
```powershell
$ ipconfig | ipconfig /all # internet interfaces  
$ route print   ## route info

## enumerating other deviced or arp enum
$ arp -a

## services & ports running ...
$ netstat -ano

## firewall configuration
>> (old ! will not work on latest )netsh firewall show state 
$ netsh advfirewall firewall # help for firewall
$ netsh advfirewall firewall show 

```


---
# Enumerating Processes & Services

1) **meterpreter commands :**
```shell
- ps                #list all running process
- pgrep lsass.exe   # grep any mention service / process
- mirgate -N lsass.exe 'OR' migrate '<id>'
```

2) **windows commands :**
```powershell
## enumerating services
$ net start
$ wmic service list brief # list of services running in background
$ tasklist /SVC  #  list of all processes running on the system along with their associated services
$ schtasks /query /fo LIST # schedule tasks information ; use /v for verbose 
$ schtasks /run /tn vulntask # run schedule tasks
```

---
# File Permissions
```powershell
# via powershell
# access control list
meterpreter > execute -f powershell.exe -H -i
PS > Get-Acl .\flag | Format-List
PS > (Get-Acl .\flag).Access
PS > $acl = Get-Acl .\flag
PS > $acl.Access | Where-Object { $_.IdentityReference -eq "NT AUTHORITY\SYSTEM" -and $_.AccessControlType -eq "Deny" }
# Remove that rule (complex unless scripted)

# cmd
# access control list
C:\> icacls flag
C:\> icacls flag /remove:d "NT AUTHORITY\SYSTEM"
```

---
# Automating Enumeration

1) **Metasploit Post Modules :** 
```shell
- post/windows/gather/win_privs # privileges enumeration
- post/windows/gather/checkvm # check if system is vm or not
- search enum_application # enum installed apps
- search enum_patches # Windows Gather Applied Patches
```

2) **Scripts :**
- [[Windows Privilege Escalation#Tools]]

---------
# Some other powershell commands
```powershell
# powershell command to execute any ps1 script (bypass execution policy)
$ powershell.exe -ExecutionPolicy Bypass -File .\script.ps1 -OutputFilename Enum.txt
		'OR'
$ powershell -ep bypass -e '<base64 encode script> OR script.ps1'

# use migrate -N explorer.exe or lsass.exe for high privelege or perform privesc 
# access smb or enumerate smb
$ net view <ip> 10.10.4.2
$ net use D: \\10.10.4.2\Documents 
$ net use K: \\10.10.4.2\K$
$ dir D:

## starting - stoping services
$ net stop wampapache    # stoping apache service
$ net start wampapache   # starting apache service

# inside phpmyadmin panel (phpmyadmin web server not in cmd)
$ [main database whcih enumerated] - *wordpress*
$ > wordpress >> wp_users >> edit > user_pass (change encryption ) >> set new password

```

---