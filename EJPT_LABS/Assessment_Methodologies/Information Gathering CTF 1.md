# Flags
1) This tells search engines what to and what not to avoid.
	- FLAG1{51727106703e4045a25e7eef706cecfc}
2) FL@G2{8677fe36e1db4a0cbb4d31b9a345383e}

3) FLAG3{0055bcf40c1b4a76893aabe33f2c6e17}

4) FLAG4{e0bc1c622cf2439da50660733361f56b}

5) FLAG5{d0a3b4f62f634e31bc020cd21c73c2b0}
# Enumeration
## Host discovery
```shell
- nmap -sn 192.12.98.1/24

Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-05-12 17:37 IST
Nmap scan report for 192.12.98.1
Host is up (0.000085s latency).
MAC Address: 02:42:62:88:6B:F6 (Unknown)
Nmap scan report for target.ine.local (192.12.98.3)
Host is up (0.000031s latency).
MAC Address: 02:42:C0:0C:62:03 (Unknown)
Nmap scan report for INE (192.12.98.2)
Host is up.
Nmap done: 256 IP addresses (3 hosts up) scanned in 1.97 seconds

```
*found target:* target.ine.local (192.12.98.3)

## robots.txt

```shell
curl http://target.ine.local/robots.txt                                                                                                                 
User-agent: *
Disallow: /wp-admin/
Allow: /wp-admin/admin-ajax.php

FLAG1{51727106703e4045a25e7eef706cecfc}
```

## Nmap Scan
```shell
sudo nmap -sSV -A -T4 target.ine.local -v | tee -a Scan.txt
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-05-12 17:42 IST
NSE: Loaded 156 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 17:42
Completed NSE at 17:42, 0.00s elapsed
Initiating NSE at 17:42
Completed NSE at 17:42, 0.00s elapsed
Initiating NSE at 17:42
Completed NSE at 17:42, 0.00s elapsed
Initiating ARP Ping Scan at 17:42
Scanning target.ine.local (192.12.98.3) [1 port]
Completed ARP Ping Scan at 17:42, 0.04s elapsed (1 total hosts)
Initiating SYN Stealth Scan at 17:42
Scanning target.ine.local (192.12.98.3) [1000 ports]
Discovered open port 80/tcp on 192.12.98.3
Completed SYN Stealth Scan at 17:42, 0.04s elapsed (1000 total ports)
Initiating Service scan at 17:42
Scanning 1 service on target.ine.local (192.12.98.3)
Completed Service scan at 17:42, 6.58s elapsed (1 service on 1 host)
Initiating OS detection (try #1) against target.ine.local (192.12.98.3)
NSE: Script scanning 192.12.98.3.
Initiating NSE at 17:42
Completed NSE at 17:42, 1.39s elapsed
Initiating NSE at 17:42
Completed NSE at 17:42, 0.13s elapsed
Initiating NSE at 17:42
Completed NSE at 17:42, 0.00s elapsed
Nmap scan report for target.ine.local (192.12.98.3)
Host is up (0.000072s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-generator: WordPress 6.5.3 - FL@G2{8677fe36e1db4a0cbb4d31b9a345383e}
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: INE
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
MAC Address: 02:42:C0:0C:62:03 (Unknown)
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.8
Uptime guess: 42.332 days (since Mon Mar 31 09:44:41 2025)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=256 (Good luck!)
IP ID Sequence Generation: All zeros

TRACEROUTE
HOP RTT     ADDRESS
1   0.07 ms target.ine.local (192.12.98.3)

NSE: Script Post-scanning.
Initiating NSE at 17:42
Completed NSE at 17:42, 0.00s elapsed
Initiating NSE at 17:42
Completed NSE at 17:42, 0.00s elapsed
Initiating NSE at 17:42
Completed NSE at 17:42, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.86 seconds
           Raw packets sent: 1023 (45.806KB) | Rcvd: 1015 (41.282KB)
```

## dir enum
Tool used : `dirsearch`
``` shell
- dirsearch -u http://target.ine.local/ -t 25 -o dir_enum

[17:44:45] 403 -  281B  - /.ht_wsr.txt                                      
[17:44:45] 403 -  281B  - /.htaccess.bak1                                   
[17:44:45] 403 -  281B  - /.htaccess.orig                                   
[17:44:45] 403 -  281B  - /.htaccess.sample
[17:44:45] 403 -  281B  - /.htaccess.save
[17:44:45] 403 -  281B  - /.htaccess_extra                                  
[17:44:45] 403 -  281B  - /.htaccess_sc
[17:44:45] 403 -  281B  - /.htaccessBAK                                     
[17:44:45] 403 -  281B  - /.htaccessOLD
[17:44:45] 403 -  281B  - /.htaccessOLD2
[17:44:45] 403 -  281B  - /.html                                            
[17:44:45] 403 -  281B  - /.htaccess_orig
[17:44:45] 403 -  281B  - /.htpasswd_test                                   
[17:44:45] 403 -  281B  - /.htpasswds                                       
[17:44:45] 403 -  281B  - /.htm                                             
[17:44:45] 403 -  281B  - /.httr-oauth                                      
[17:44:46] 403 -  281B  - /.php                                             
[17:45:12] 301 -    0B  - /index.php  ->  http://target.ine.local/          
[17:45:12] 404 -   56KB - /index.php/login/                                 
[17:45:27] 200 -  110B  - /robots.txt                                       
[17:45:28] 403 -  281B  - /server-status                                    
[17:45:28] 403 -  281B  - /server-status/
[17:45:40] 301 -  323B  - /wp-admin  ->  http://target.ine.local/wp-admin/  
[17:45:40] 302 -    0B  - /wp-admin/  ->  http://target.ine.local/wp-login.php?redirect_to=http%3A%2F%2Ftarget.ine.local%2Fwp-admin%2F&reauth=1
[17:45:40] 400 -    1B  - /wp-admin/admin-ajax.php                          
[17:45:40] 409 -    3KB - /wp-admin/setup-config.php                        
[17:45:40] 200 -    3KB - /wp-config.bak                                    
[17:45:40] 200 -  489B  - /wp-admin/install.php                             
[17:45:40] 200 -    0B  - /wp-config.php                                    
[17:45:40] 301 -  325B  - /wp-content  ->  http://target.ine.local/wp-content/
[17:45:40] 200 -    0B  - /wp-content/                                      
[17:45:41] 200 -   84B  - /wp-content/plugins/akismet/akismet.php           
[17:45:41] 500 -    0B  - /wp-content/plugins/hello.php                     
[17:45:41] 200 -  509B  - /wp-content/uploads/                              
[17:45:41] 301 -  326B  - /wp-includes  ->  http://target.ine.local/wp-includes/
[17:45:41] 200 -    0B  - /wp-includes/rss-functions.php
[17:45:41] 200 -    0B  - /wp-cron.php
[17:45:41] 200 -    5KB - /wp-includes/                                     
[17:45:41] 200 -    2KB - /wp-login.php                                     
[17:45:41] 302 -    0B  - /wp-signup.php  ->  http://target.ine.local/wp-login.php?action=register
[17:45:42] 405 -   42B  - /xmlrpc.php 
```

### wp-content/upload
```shell
curl http://target.ine.local/wp-content/uploads/flag.txt
### browse this upload dir  in firefox and look for flag
```
### backup
```shell
curl http://target.ine.local/wp-config.bak

<?php
/**
 * The base configuration for WordPress
 *
 * The wp-config.php creation script uses this file during the installation.
 * You don't have to use the website, you can copy this file to "wp-config.php"
 * and fill in the values.
 *
 * This file contains the following configurations:
 *
 * * Database settings
 * * Secret keys
 * * Database table prefix
 * * ABSPATH
 *
 * @link https://wordpress.org/documentation/article/editing-wp-config-php/
 *
 * @package WordPress
 */

// ** Database settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'test' );

/** Database username */
define( 'DB_USER', 'test' );

/** Database password */
define( 'DB_PASSWORD', 'test' );

/** Database hostname */
define( 'DB_HOST', 'localhost' );

/** Database charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The database collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );

/**#@+
 * Authentication unique keys and salts.
 *
 * Change these to different unique phrases! You can generate these using
 * the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}.
 *
 * You can change these at any point in time to invalidate all existing cookies.
 * This will force all users to have to log in again.
 *
 * @since 2.6.0
 */
define('AUTH_KEY',         '}Mq^#|v{n0fQ6Vn[tr 6e4glzi:OVs/9(IQ .7f^dp3ym4,th-O$Qx.]|2+(t(sE');
define('SECURE_AUTH_KEY',  'S_LKQ#*}p*U}kdX[GNNVM2*0YISNQ&zrFl jEUNq5T}0Zg|,sO|yB68^|N*1nS-p');
define('LOGGED_IN_KEY',    '`tz-Uz9Ixka,5z0J BD0l/zfU|r2|;9BGL5l~A1RQtZMwh=JftaU$2)$FI%v};|E');
define('NONCE_KEY',        '|>ZN961k>aHWJ*R8#&x+rR>3g|<[:G 8B+rqPH WrWet1SC60+ LL/S+=[G-&g7)');
define('AUTH_SALT',        '+<2l=;osCL(L)zV[=uvr[}2^j-16(gFq18V<m|fP<R{7DV`^=O&bb3fxY+Jf|-;C');
define('SECURE_AUTH_SALT', 'HG&/Q/ceR-$;?jCL}<cL4@LKzDjv,M=K-gR<]iHiAqcHQO+rXcWn/jMt0#K,uWq%');
define('LOGGED_IN_SALT',   'REsFv+OsL*qd=yV<oPaAXeYj@f)A[/Wm5-?|_4d::(;dXcps`rgJf]t4B0Q3)RcH');
define('NONCE_SALT',       ' Q.:O=pFDTA-lNBe%kjJu(mp7$cQrF|IZ _hOWDA&Q18w6CL(<{+1$a-ZJ~<(!_;');


/** FLAG4{e0bc1c622cf2439da50660733361f56b} */


/**#@-*/

/**
 * WordPress database table prefix.
 *
 * You can have multiple installations in one database if you give each
 * a unique prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';

/**
 * For developers: WordPress debugging mode.
 *
 * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 *
 * For information on other constants that can be used for debugging,
 * visit the documentation.
 *
 * @link https://wordpress.org/documentation/article/debugging-in-wordpress/
 */
define( 'WP_DEBUG', false );

/* Add any custom values between this line and the "stop editing" line. */



/* That's all, stop editing! Happy publishing. */

/** Absolute path to the WordPress directory. */
if ( ! defined( 'ABSPATH' ) ) {
        define( 'ABSPATH', __DIR__ . '/' );
}

/** Sets up WordPress vars and included files. */
require_once ABSPATH . 'wp-settings.php';
define('WP_HOME', 'http://X.X.X.X');

define('WP_SITEURL', 'http://X.X.X.X');

define('WP_HTTP_BLOCK_EXTERNAL', true);

```

## web mirror
```shell
# mirror website
- httrack --mirror http://target.ine.local/ 
- cd target.ine.local
- cat * | grep FLAG5
-> <api name="FLAG5{d0a3b4f62f634e31bc020cd21c73c2b0}" blogID="1" preferred="false" apiLink="http://target.ine.local/xmlrpc.php" />
 
```