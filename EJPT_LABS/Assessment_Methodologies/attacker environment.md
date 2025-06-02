# lab configuration

- *Set numbers of workspaces*: 5 
- *Set terminal config*: 
	-> right click to terminal > preference > *font:15* and *color scheme:GreenonBlack* >> Enter
- if needed use zsh 
### *ifconfig*
```shell
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.1.0.5  netmask 255.255.0.0  broadcast 10.1.255.255
        ether 02:42:0a:01:00:05  txqueuelen 0  (Ethernet)
        RX packets 8902  bytes 707036 (690.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 7217  bytes 3243204 (3.0 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.12.98.2  netmask 255.255.255.0  broadcast 192.12.98.255
        ether 02:42:c0:0c:62:02  txqueuelen 0  (Ethernet)
        RX packets 18  bytes 1516 (1.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 21813  bytes 18983837 (18.1 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 21813  bytes 18983837 (18.1 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```