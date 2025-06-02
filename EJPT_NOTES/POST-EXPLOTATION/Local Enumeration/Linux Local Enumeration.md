# Enumerating System Information

```shell
# basic commands...
- uname -a | uname -r 
- hostname
- cat /etc/issue
- cat /etc/os-release
- env # environment details
- lscpu # cpu info
- df -h # file system usage
- lsblk # list block devices
- dpkg -l # list installed packages 

## using find utility enumerating other executable services that has root permission

- find / -not -type l -perm -o+w   # Find files writable by others
- find -rnw /usr -e "/home/user/file"  # search file in recursive 
- find / -name '*.sh' # for searching crontab file 
- find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null    # Find all SUID/SGID executables 
- find / -perm /4000 2>/dev/null # another command for suid exec.. 

```

---

# Enumerating Users & Groups

```shell
## meterpreter commands ...
- getuid

## shell commands....
- whoami
- id 
- groups root 'OR' any user name   # print the groups a user is in
- cat /etc/passwd # enumerate others users
- cat /etc/group  # enumerate group users 
- w , who , last , lastlog ## commands for enumerating loggon users
```

---
# Enumerating Network Information

```shell
## meterpreter commands...
- ifconfig    # ip address - subnetmask
- netstat     # currently running tcp/udp services 
- route       # route info
- arp         # arp 

## shell commands...
- ifconfig 'OR' ip a s 'OR' cat /etc/networks  # get network info
- cat /etc/hostname 'OR' hostname # get hostname
- cat /etc/hosts # get internal host/domain info
- cat /etc/resolv.conf  # get current dns nameserver info
- arp -a    # for viewing and troubleshooting connected devices 

```

---
# Enumerating  Processes & Cron Jobs

```shell
## meterpreter commands...
- ps 
- pgrep

## shell commands...
- ps aux   # list current running process
- netstat -puntl # list currently listening servers / services (tcp/udp)
- netstat -antp  # currently listening / ESTABLISHED connections / services (all)
- ss -tnl

## for crontab
- crontab -l 
- ls -la /etc/cron*
- cat /etc/cron*

```

---
# Automating Enumeration
- *Tools:* [[Linux Privilege Escalation#Tools]]
---
# Other usefull commands
``` shell
# if /etc/shadow has writable permission then do modify to gain root shell
- openssl passwd -1 -salt abc password # OpenSSL Password Hashing Command 
				OR

# generate new password hash
- mkpasswd -m sha-512 newpasswordhere

# create user resemble to any service
- useradd -m ftp -s /bin/bash # create user
- passwd ftp >> add passwd    # add password to user
- usermod -aG root ftp        # add new user to root group

# group  
- groups ftp                  # check user by using..
- usermod -u 15 ftp           # change id number
```