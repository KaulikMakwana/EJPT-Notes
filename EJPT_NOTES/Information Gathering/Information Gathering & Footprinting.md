### **Types of Information Gathering:** 

1) *Passive Information Gathering:*
	-> gather information by `without actively engaging` with the target.
	- ip address & dns info
	- domain names , social profiles
	- subdomains, web technology, google dorks
	- search engines (shodan, cencys,netcraft,dnsdumpster,other similar..)

2) *Active Information Gathering:*
	-> gather information by `actively engaging with the target`system.
	- port scanning, service/ os fingerprinting 
	- host discovery/ network mapping
	- internal infrastructure
	- enumeration

---
### ✅ **Passive Information Gathering Tools**

|           *Tool*           |                              *Description*                              |
| :------------------------: | :---------------------------------------------------------------------: |
|           *whois*            |       Gets domain registration data from public WHOIS databases.        |
|          *netcraft*          |     Provides hosting, tech stack, OS info from its public database.     |
|        *theHarvester*        | Collects emails, subdomains, usernames from public sources like Google. |
|           *shodan*           |   Searches internet-exposed devices using Shodan’s pre-scanned data.    |
|           *censys*           |        Same as Shodan—searches pre-collected internet scan data.        |
|        *DNSDumpster*         |            Uses public DNS records and passive data sources.            |
|           *crt.sh*           |         Retrieves SSL certs from Certificate Transparency logs.         |
|       *Google Dorking*       |    Uses Google Search to find exposed data, files, directories, etc.    |
|         *Sublist3r*          |        Queries search engines and public sources for subdomains.        |
|       *FOCA (passive)*       |                Extracts metadata from public documents.                 |
| *recon-ng (passive modules)* |   Uses WHOIS, APIs, social media, etc., without touching the target.    |
|    *Amass (passive mode)*    |    Uses public APIs and third-party sources for subdomain discovery.    |
### 🔥 **Active Information Gathering Tools**

|          *Tool*           |                          *Description*                           |
| :-----------------------: | :--------------------------------------------------------------: |
|           *host*            |              Direct DNS query to resolve domain/IP.              |
|            *dig*            |            Sends DNS queries directly to DNS servers.            |
|         *dnsrecon*          |  Actively attempts DNS zone transfers, brute-force subdomains.   |
|          *dnsenum*          |        Similar to dnsrecon, with aggressive DNS probing.         |
|           *Nmap*            | Scans ports and services—interacts directly with target systems. |
|           *Nikto*           |           Scans web servers for known vulnerabilities.           |
|          *WhatWeb*          |        Sends HTTP requests to identify web technologies.         |
|      *Wappalyzer CLI*       |      Makes HTTP requests to detect technologies on a site.       |
|        *Burp Suite*         |    Intercepts, modifies, and sends web requests—very active.     |
|          *Nuclei*           |       Sends crafted HTTP requests to find vulnerabilities.       |
|    *Amass (active mode)*    |        Performs DNS resolution, brute-force, and probing.        |
| *recon-ng (active modules)* |               Performs DNS brute, port scans, etc.               |
|       *FOCA (active)*       |        Does DNS and network interaction for deeper recon.        |

---

