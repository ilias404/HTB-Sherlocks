# CrownJewel-2

Sherlock Scenario

> Forela's Domain environment is pure chaos. Just got another alert from the Domain controller of NTDS.dit database being exfiltrated. Just one day prior you responded to an alert on the same domain controller where an attacker dumped NTDS.dit via vssadmin utility. However, you managed to delete the dumped files kick the attacker out of the DC, and restore a clean snapshot. Now they again managed to access DC with a domain admin account with their persistent access in the environment. This time they are abusing ntdsutil to dump the database. Help Forela in these chaotic times!!

# Task 1: When utilizing ntdsutil.exe to dump NTDS on disk, it simultaneously employs the Microsoft Shadow Copy Service. What is the most recent timestamp at which this service entered the running state, signifying the possible initiation of the NTDS dumping process?

In `SYSTEM.evtx`, events related to the Microsoft Shadow Copy Service were searched for to identify when the service entered the running state.

<img width="1046" height="274" alt="image" src="https://github.com/user-attachments/assets/d74bb890-034b-4a94-a246-d65d818d4359" />

The XML view of the event was then examined to verify the exact timestamp.

<img width="1087" height="428" alt="image" src="https://github.com/user-attachments/assets/e6b6f9df-5eb3-4db0-800f-4f07181a93d7" />

Ans: `2024-05-15 05:39:55`

# Task 2: Identify the full path of the dumped NTDS file.

In `APPLICATION.evtx`, filtering around the previously identified timestamp `2024-05-15 05:39:55` revealed several interesting events related to the NTDS dumping activity. These events provide information that can be used to identify the full path of the dumped NTDS file.

<img width="1169" height="368" alt="image" src="https://github.com/user-attachments/assets/8a3ef335-e858-410f-b485-22aaafbdc2d8" />


Ans: `C:\Windows\Temp\dump_tmp\Active Directory\ntds.dit`

# Task 3: When was the database dump created on the disk?

In the same event, the timestamp indicating when the database dump was created on the disk can be observed.

<img width="490" height="321" alt="image" src="https://github.com/user-attachments/assets/bf960057-f7d0-4c2e-b225-d7882ba281ab" />

Ans: `2024-05-15 05:39:56`

# Task 4: When was the newly dumped database considered complete and ready for use?

The database dump is considered complete and ready for use once it is detached by the database engine.

<img width="1292" height="409" alt="image" src="https://github.com/user-attachments/assets/2a29920e-9da5-4f28-9bb7-e9883d76178b" />

Looking at the same event, the timestamp corresponding to the detachment can be identified.

<img width="512" height="261" alt="image" src="https://github.com/user-attachments/assets/12148e70-35e8-4129-8d1e-2704e7cb9388" />

Ans: `2024-05-15 05:39:58`

# Task 5: Event logs use event sources to track events coming from different sources. Which event source provides database status data like creation and detachment?

<img width="882" height="240" alt="image" src="https://github.com/user-attachments/assets/4c1b9a8f-bbb3-47df-ae5d-be2e7840732c" />

Ans: `ESENT`

# Task 6: When ntdsutil.exe is used to dump the database, it enumerates certain user groups to validate the privileges of the account being used. Which two groups are enumerated by the ntdsutil.exe process? Give the groups in alphabetical order joined by comma space.

In Security.evtx, filtering for **Event ID 4799** can help identify events related to security-enabled local group membership enumeration.

<img width="421" height="325" alt="image" src="https://github.com/user-attachments/assets/79b6bf69-f5af-4f5b-868c-c33964f13e4c" />
<img width="436" height="363" alt="image" src="https://github.com/user-attachments/assets/c7e64d69-b1ab-44b5-b4a5-86bcc26cf92d" />

The two we found are:

Ans: `Administrators, Backup Operators`


# Task 7: Now you are tasked to find the Login Time for the malicious Session. Using the Logon ID, find the Time when the user logon session started.



Ans: `ESENT`







