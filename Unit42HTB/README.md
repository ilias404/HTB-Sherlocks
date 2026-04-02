# Unit42 HTB

![unit42.jpg](/Unit42HTB/screenshots/unit42.jpg)

# Sherlock Scenario
> In this Sherlock, you will familiarize yourself with Sysmon logs and various useful EventIDs for identifying and analyzing malicious activities on a Windows system. Palo Alto's Unit42 recently conducted research on an UltraVNC campaign, wherein attackers utilized a backdoored version of UltraVNC to maintain access to systems. This lab is inspired by that campaign and guides participants through the initial access stage of the campaign.

# Task 1: How many Event logs are there with Event ID 11?

Filtering for event ID 11 reveals:

![eventid11.png](/Unit42HTB/screenshots/eventid11.png)

> Sysmon Event ID 11: File Create
> 
> This event is generated when a file is created or overwritten on the system. It records details such as the process responsible for the action and the full path of the created file. Event ID 11 is especially useful for detecting malware drops, as attackers often write payloads into directories like AppData, Temp, or Downloads. By analyzing this event, analysts can identify suspicious file creation activity and understand what artifacts a process has introduced onto the system.

Ans: `56`

# Task 2: Whenever a process is created in memory, an event with Event ID 1 is recorded with details such as command line, hashes, process path, parent process path, etc. This information is very useful for an analyst because it allows us to see all programs executed on a system, which means we can spot any malicious processes being executed. What is the malicious process that infected the victim's system?

Filtering for event ID 1 reveals the following:

![preventivo.png](/Unit42HTB/screenshots/preventivo.png)

> Sysmon Event ID 1: Process Creation
> 
> This event is generated whenever a new process is started on the system. It provides detailed information such as the process name, full executable path, command line arguments, parent process, user context, and process hash values. Event ID 1 is one of the most important logs for detecting malicious activity, as it allows analysts to identify suspicious executions, unusual parent-child process relationships, and the use of scripts or tools commonly associated with attacks.

Ans: `C:\Users\CyberJunkie\Downloads\Preventivo24.02.14.exe.exe`

# Task 3: Which Cloud drive was used to distribute the malware?

Since the malware is distributed via a cloud drive, analyzing DNS activity is important. Sysmon Event ID 22 (DNS Query) logs all domain requests made by processes, helping identify suspicious or malicious domains used to download the malware.

![example.png](/Unit42HTB/screenshots/example.png)

> Sysmon Event ID 22: DNS Query
> 
> This event logs all DNS requests made by processes on the system, including information about the querying process and the requested domain. This information is useful for identifying suspicious or malicious domains that malware may contact to download additional payloads or communicate with command-and-control servers. It can help to trace network activity relating to an attack.
