# CrownJewel-2

Sherlock Scenario

> Forela's Domain environment is pure chaos. Just got another alert from the Domain controller of NTDS.dit database being exfiltrated. Just one day prior you responded to an alert on the same domain controller where an attacker dumped NTDS.dit via vssadmin utility. However, you managed to delete the dumped files kick the attacker out of the DC, and restore a clean snapshot. Now they again managed to access DC with a domain admin account with their persistent access in the environment. This time they are abusing ntdsutil to dump the database. Help Forela in these chaotic times!!

# Task 1: When utilizing ntdsutil.exe to dump NTDS on disk, it simultaneously employs the Microsoft Shadow Copy Service. What is the most recent timestamp at which this service entered the running state, signifying the possible initiation of the NTDS dumping process?

In `SYSTEM.evtx`, events related to the Microsoft Shadow Copy Service were searched for to identify when the service entered the running state.

<img width="1046" height="274" alt="image" src="https://github.com/user-attachments/assets/d74bb890-034b-4a94-a246-d65d818d4359" />

The XML view of the event was then examined to verify the exact timestamp.

<img width="1087" height="428" alt="image" src="https://github.com/user-attachments/assets/e6b6f9df-5eb3-4db0-800f-4f07181a93d7" />

Ans: `2024-05-15 05:39:55`












