# CrownJewel-1

# Sherlock Scenario

> Forela's domain controller is under attack. The Domain Administrator account is believed to be compromised, and it is suspected that the threat actor dumped the NTDS.dit database on the DC. We just received an alert of vssadmin being used on the DC, since this is not part of the routine schedule we have good reason to believe that the attacker abused this LOLBIN utility to get the Domain environment's crown jewel. Perform some analysis on provided artifacts for a quick triage and if possible kick the attacker as early as possible.

We begin by extracting the provided artifacts. The evidence consists of three `.evtx` event log files and an `$MFT` file.

> In Windows NTFS drives, $MFT stands for the Master File Table. It is a hidden, foundational system database where the operating system stores metadata—such as file size, time stamps, permissions, and physical disk locations—for every single file and folder on that partition.

The EVTX files will be used to investigate authentication, process execution, and other system activity, while the $MFT will be used to examine filesystem activity and correlate file creation or modification timestamps with events observed in the logs.

# Task 1: Attackers can abuse the vssadmin utility to create volume shadow snapshots and then extract sensitive files like NTDS.dit to bypass security mechanisms. Identify the time when the Volume Shadow Copy service entered a running state.

Since `SYSTEM.evtx` records events related to Windows services and their state changes, we can search it for the `Volume Shadow Copy Service (VSS)` and identify when it entered the running state.

<img width="1309" height="260" alt="image" src="https://github.com/user-attachments/assets/0dd4f401-d9e3-4d0c-9dc3-67ea3d247cc0" />

We find a record of the Volume Shadow Copy Service entering the running state. By switching to the XML view, we can obtain the exact timestamp in UTC.

<img width="910" height="364" alt="image" src="https://github.com/user-attachments/assets/58f062cb-8494-44f9-85de-d824bf045cb5" />

Ans: `2024-05-14 03:42:16`

# Task 2: When a volume shadow snapshot is created, the Volume shadow copy service validates the privileges using the Machine account and enumerates User groups. Find the two user groups the volume shadow copy process queries and the machine account that did it.



Ans: ``






