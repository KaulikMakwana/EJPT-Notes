```mermaid
flowchart TD
    A[🕵️ 1. Information Gathering] --> B1[🧠 Passive]
    A --> B2[⚡ Active]

    B1 --> C1[🌐 OSINT<br/>whois, theHarvester, Shodan, Censys]
    B1 --> C2[🔍 Public DNS<br/>DNSDumpster, crt.sh]
    B2 --> C3[🧭 Network Map<br/>netdiscover, arp-scan]
    B2 --> C4[🚪 Port Scan<br/>nmap, masscan]
    B2 --> C5[🔍 Service & OS Detect<br/>WhatWeb, Wappalyzer]

    C1 --> D[📦 2. Enumeration]
    C2 --> D
    C3 --> D
    C4 --> D
    C5 --> D

    D --> E1[🧑‍💻 Service/User/Share<br/>enum4linux, smbmap, rpcclient]

    E1 --> F[💥 3. Exploitation]

    F --> G1[🧪 Vulnerability Analysis<br/>Nuclei, searchsploit, OpenVAS]
    F --> G2[🚨 Exploitation<br/>Metasploit, RCE, SQLi, custom exploits]

    G1 --> H[🔐 4. Post-Exploitation]
    G2 --> H

    H --> I1[🔍 Local Enum<br/>linPEAS, winPEAS]
    H --> I2[📈 Priv Esc<br/>kernel exploits, SUID]
    H --> I3[🧬 Credential Dump<br/>mimikatz, hashdump]
    H --> I4[🧠 Lateral Movement<br/>psexec, RDP, BloodHound]
    H --> I5[🔁 Persistence<br/>Services, crontab, registry]

    I1 --> J[🧾 5. Reporting]
    I2 --> J
    I3 --> J
    I4 --> J
    I5 --> J

    J --> K[📝 Report<br/>PoC, Summary, Fixes]
