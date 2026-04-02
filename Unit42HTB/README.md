# Unit42 HTB

![unit42.jpg](/Unit42HTB/screenshots/unit42.jpg)

# Sherlock Scenario
> In this Sherlock, you will familiarize yourself with Sysmon logs and various useful EventIDs for identifying and analyzing malicious activities on a Windows system. Palo Alto's Unit42 recently conducted research on an UltraVNC campaign, wherein attackers utilized a backdoored version of UltraVNC to maintain access to systems. This lab is inspired by that campaign and guides participants through the initial access stage of the campaign.

# Task 1: How many Event logs are there with Event ID 11?

Filtering for event ID 11 reveals:

![eventid11.png](/Unit42HTB/screenshots/eventid11.pmg)

> Sysmon Event ID 11: File Create
> This event is generated when a file is created or overwritten on the system. It records details such as the process responsible for the action and the full path of the created file. Event ID 11 is especially useful for detecting malware drops, as attackers often write payloads into directories like AppData, Temp, or Downloads. By analyzing this event, analysts can identify suspicious file creation activity and understand what artifacts a process has introduced onto the system.
