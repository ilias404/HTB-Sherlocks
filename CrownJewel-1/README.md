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

In `SECURITY.evtx`, we can investigate events around the timestamp identified in the previous task `2024-05-14 03:42:16`.

<img width="1311" height="549" alt="image" src="https://github.com/user-attachments/assets/c00927fd-0323-484a-bc86-938bb37d2672" />

We find several Event ID `4799` entries.

> Windows Event ID 4799 is logged in the Security event log when a process or user checks (enumerates) the members of a security-enabled local group. It details who requested the check, which group was looked at, and the specific application or process used to do it.

Reviewing these events, we identify the following two group enumeration events:

<img width="459" height="282" alt="image" src="https://github.com/user-attachments/assets/bc70bcce-23af-48ae-ba2c-6e386f106798" />

<img width="449" height="278" alt="image" src="https://github.com/user-attachments/assets/5f3af8c9-2f83-4f59-bf98-fa68f4fe968d" />

Ans: `Administrators, Backup Operators, DC01$`

# Task 3: Identify the Process ID (in Decimal) of the volume shadow copy service process.

Selecting one of the relevant Event ID `4799` events, we can find the Process ID. The PID is displayed in hexadecimal.

<img width="1240" height="427" alt="image" src="https://github.com/user-attachments/assets/198a2f05-e208-475a-bbbd-00783a1ff835" />

After converting the hexadecimal Process ID to decimal, we obtain:

<img width="788" height="153" alt="image" src="https://github.com/user-attachments/assets/59b3415f-2b7a-49d3-8d11-35f8fa6c5e24" />

Ans: `4496`

# Task 4: Find the assigned Volume ID/GUID value to the Shadow copy snapshot when it was mounted.

Reviewing `Microsoft-Windows-NTFS.evtx` and filtering for events occurring after `2024-05-14 03:42:16`, we identify the following:

<img width="1285" height="453" alt="image" src="https://github.com/user-attachments/assets/1bf4fc4e-3a96-44d8-a83d-bcae31f372ce" />

Ans: `{06c4a997-cca8-11ed-a90f-000c295644f9}`

# Task 5: Identify the full path of the dumped NTDS database on disk.

> `NTDS.dit` is the main database file for Microsoft Active Directory. It lives on domain controllers and stores user accounts, group details, and password hashes.

From Task 4, we identified the timestamp at which the NTFS volume was mounted: `2024-05-14 04:44:22`. We can use this timestamp as a reference point when investigating filesystem activity related to the Volume Shadow Copy operation.

For this task, we use the provided `$MFT` artifact to identify `.dit` files present on the filesystem.

Using **MFTECmd**, we parse the `$MFT` file and export the results for further analysis.

```powershell
.\MFTECmd.exe -f "C:\Users\lenovo\Desktop\CrownJewel1\Artifacts\C\`$MFT" --csv "C:\Users\lenovo\Desktop\CrownJewel1" --csvf CrownJewel1MFT.csv
```

The resulting output allows us to search for `NTDS.dit` and identify the full path of the dumped database.

<img width="1357" height="228" alt="image" src="https://github.com/user-attachments/assets/4ec7a7da-d2cd-4e78-ab04-809ba4a756a4" />

Ans: `C:\Users\Administrator\Documents\backup_sync_Dc\Ntds.dit`

# Task 6: When was newly dumped ntds.dit created on disk?

The timestamp can be identified in the final screenshot of Task 5.

Ans: `2024-05-14 03:44:22`

# Task 7: A registry hive was also dumped alongside the NTDS database. Which registry hive was dumped and what is its file size in bytes?

We can further narrow down the results by filtering for the same path, `\Users\Administrator\Documents\backup_sync_Dc\`, identified in the previous task.

<img width="836" height="129" alt="image" src="https://github.com/user-attachments/assets/0c56e69a-bdd2-42e4-9935-0d9399166c2e" />

Ans: `SYSTEM, 17563648`

