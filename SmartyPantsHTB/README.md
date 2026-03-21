# SmartyPants

![smartypants.png](/SmartyPantsHTB/screenshots/smartypants.png)

# Sherlock Scenario
> Forela's CTO, Dutch, stores important files on a separate Windows system because the domain environment at Forela is frequently breached due to its exposure across various industries. On 24 January 2025, our worst fears were realised when an intruder accessed the fileserver, installed utilities to aid their actions, stole critical files, and then deleted them, rendering them unrecoverable. The team was immediately informed of the extortion attempt by the intruders, who are now demanding money. While our legal team addresses the situation, we must quickly perform triage to assess the incident's extent. Note from the manager: We enabled SmartScreen Debug Logs across all our machines for enhanced visibility a few days ago, following a security research recommendation. These logs can provide quick insights, so ensure they are utilised.

# Task 1: The attacker logged in to the machine where Dutch saves critical files, via RDP on 24th January 2025. Please determine the timestamp of this login.

Unzipping the files reveals a ton of ```.evtx``` files.
> ```.evtx``` files are Windows Event Logs that record system, security, and application events, like logins, service starts, or errors. They help admins and analysts troubleshoot issues and investigate suspicious activity, and can be read in Event Viewer, PowerShell, or with tools like ```EvtxECmd```.

We will use [```Timeline Explorer```](https://ericzimmerman.github.io/#!index.md) and [```EvtxECmd```](https://ericzimmerman.github.io/#!index.md).

We navigate to the directory where ```Evtxecmd``` is installed and run the following: 

```bash
.\EvtxECmd.exe -d "C:\Users\lenovo\Downloads\SmartyPants\Logs" --csv output.csv
```
![evtxecmd.png](/SmartyPantsHTB/screenshots/evtxecmd.png)

We can now open the output using the second tool, ```Timeline Explorer```.

When we filter for event log number 1149, we find the following:
> Event ID 1149 logs RDP authentication attempts in Windows. It shows which user and IP tried to connect, but does not confirm a successful login. Analysts use it to detect RDP access attempts or brute-force activity, often correlating it with Event ID 4624 for actual logins.

![1149.png](/SmartyPantsHTB/screenshots/1149.png)

Answer: ```2025-01-24 10:22:19```

