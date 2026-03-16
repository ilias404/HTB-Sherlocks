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
