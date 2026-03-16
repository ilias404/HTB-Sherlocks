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

After a simple Google search, we found the following:

> CVE-2026-24061 is a critical authentication bypass in GNU InetUtils telnetd. It affects versions 1.9.3 through 2.7 and carries a CVSS 9.8 severity rating.
At a high level, the Telnet daemon passes an attacker controlled USER value into the system login process without properly sanitizing it, which can lead to argument injection and an authentication bypass that results in root access.

Answer: ```CVE-2026-24061```

# Task 2: When was the Telnet vulnerability successfully exploited, granting the attacker remote root access on the target machine?

Right-clicking packet number 25 and selecting ```Follow > TCP Stream``` should allow us to see the entire conversation between the attacker and the system:

![firstexploit.png](/TellyHTB/screenshots/firstexploit1.png)

We will click ```Save as``` to save this conversation locally for later use.

Answer: ```2026-01-27 10:39:28```

# Task 3: What is the hostname of the targeted server?

```bash
cat terminal_log.txt | sed 's/root@backup-secondary/\x1b[31m&\x1b[0m/g'
```

![backupsecondary.png](/TellyHTB/screenshots/backupsecondary.png)

Answer: ```backupsecondary```

