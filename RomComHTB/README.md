# RomCom

![romcom.png](/RomComHTB/screenshots/romcom.png)

# Sherlock Scenario
> Susan works at the Research Lab in Forela International Hospital. A Microsoft Defender alert was received from her computer, and she also mentioned that while extracting a document from the received file, she received tons of errors, but the document opened just fine. According to the latest threat intel feeds, WinRAR is being exploited in the wild to gain initial access into networks, and WinRAR is one of the Software programs the staff uses. You are a threat intelligence analyst with some background in DFIR. You have been provided a lightweight triage image to kick off the investigation while the SOC team sweeps the environment to find other attack indicators.

# Task 1: What is the CVE assigned to the WinRAR vulnerability exploited by the RomCom threat group in 2025?

A Google search leads us to the following: 

![cve.png](/RomComHTB/screenshots/cve.png)

Answer: ```CVE-2025-8088```

# Task 2: What is the nature of this vulnerability?

Answer: ```Path Traversal```

# Task 3: What is the name of the archive file under Susan's documents folder that exploits the vulnerability upon opening the archive file?

Let's start by extracting the ```RomCom.zip``` file.

> Note: We will be using a Windows machine to solve this Sherlock.

After extraction, we end up with a ```.vhdx``` file.

> A ```.vhdx``` file is a virtual hard drive. It’s a file that holds an entire computer’s disk, including system files, programs, and documents. We can open or mount it to look at the files without changing the original.

On our Windows machine, let's ```Right-click > Mount``` to see what's inside.

![mount.png](/RomComHTB/screenshots/mount.png)

We end up with two important files: ```$MFT``` and ```$J```.
> ```$MFT``` (Master File Table): The main database of an NTFS disk. It stores metadata for every file and folder, like names, sizes, and timestamps, letting you see what exists or existed on the system.
```$J``` (USN Journal): A log of all file changes on the disk, including creation, modification, deletion, and renaming. Even deleted files can appear here, making it useful for tracking activity over time.
