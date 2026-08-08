# Reaper
![rp.png](/Reaper/screenshots/rp.png)

# Sherlock Scenario
> Our SIEM alerted us to a suspicious logon event which needs to be looked at immediately . The alert details were that the IP Address and the Source Workstation name were a mismatch .You are provided a network capture and event logs from the surrounding time around the incident timeframe. Corelate the given evidence and report back to your SOC Manager.

# Task 1: What is the IP Address for Forela-Wkstn001?

By examining the first packet in Wireshark, we can identify the IP address assigned to Forela-Wkstn001.

![ip.png](/Reaper/screenshots/ip.png)

Ans: `172.17.79.129`

# Task 2: What is the IP Address for Forela-Wkstn002?

We can identify the IP address of Forela-Wkstn002 by navigating to `Edit > Find Packet`, setting the search type to `String`, and searching for the workstation's hostname.
Alternatively, we can filter for the `NBNS (NetBIOS Name Service)` protocol.
> `NBNS (NetBIOS Name Service)` is a protocol used by Windows systems to resolve NetBIOS hostnames to IP addresses on a local network. By examining the relevant NBNS packets, we can determine the IP address associated with Forela-Wkstn002.

![ip2.png](/Reaper/screenshots/ip2.png)

Ans: `172.17.79.136`

# Task 3: What is the username of the account whose hash was stolen by attacker?


