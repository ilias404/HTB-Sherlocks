# BFT

<img width="571" height="314" alt="image" src="https://github.com/user-attachments/assets/3de60a79-1de3-4f84-8ce2-fc228cbeadd0" />


# Sherlock Scenario

> Sherlock Overview:
>
> In this Sherlock, you will become acquainted with MFT (Master File Table) forensics. You will be introduced to well-known tools and methodologies for analyzing MFT artifacts to identify malicious activity. During our analysis, you will utilize the MFTECmd tool to parse the provided MFT file, TimeLine Explorer to open and analyze the results from the parsed MFT, and a Hex editor to recover file contents from the MFT.
>
> Tools Used:
> MFTECmd
> TimeLine Explorer
> HxD Hex Editor
> MFTECmd.exe -f "C:\Users\CyberJunkie\Desktop\C\\$MFT" --csv "C:\Users\CyberJunkie\Desktop\" --csvf MFT_ANALYSIS.csv
>
> The above command processes the MFT file located in "C:\Users\CyberJunkie\Desktop\C" and creates a CSV file named MFT_ANALYSIS.csv on the Desktop of the user CyberJunkie.
>
> Note: You will need to replace the file paths with your own.
>
> Next, open the CSV file in TimeLine Explorer to begin your analysis.


# Task 1: Simon Stark was targeted by attackers on February 13. He downloaded a ZIP file from a link received in an email. What was the name of the ZIP file he downloaded from the link?

<img width="1103" height="370" alt="image" src="https://github.com/user-attachments/assets/2a1f877e-fcee-45ec-8e91-c8c23f22ac2a" />

After applying some filters:

<img width="582" height="311" alt="image" src="https://github.com/user-attachments/assets/4794cbf1-9e56-498b-a38f-2099d5b86781" />

<img width="904" height="147" alt="image" src="https://github.com/user-attachments/assets/561686e4-7826-4752-82a1-db540702b9b4" />

Based on the timestamps and the surrounding MFT artifacts, Stage-20240213T093324Z-001.zip is identified as the ZIP file downloaded by the victim.

Ans: `Stage-20240213T093324Z-001.zip`

# Task 2: Examine the Zone Identifier contents for the initially downloaded ZIP file. This field reveals the HostUrl from where the file was downloaded, serving as a valuable Indicator of Compromise (IOC) in our investigation/analysis. What is the full Host URL from where this ZIP file was downloaded?

> A Zone.Identifier is a hidden piece of metadata that Windows can attach to files downloaded from the Internet

<img width="631" height="679" alt="image" src="https://github.com/user-attachments/assets/a21ecd0f-c1d6-475e-9e3d-4ebd817debba" />

Ans: `https://storage.googleapis.com/drive-bulk-export-anonymous/20240213T093324.039Z/4133399871716478688/a40aecd0-1cf3-4f88-b55a-e188d5c1c04f/1/c277a8b4-afa9-4d34-b8ca-e1eb5e5f983c?authuser`

# Task 3: What is the full path and name of the malicious file that executed malicious code and connected to a C2 server?

<img width="1274" height="29" alt="image" src="https://github.com/user-attachments/assets/2634bb1b-1df2-49bf-853e-a176e1b70d13" />


Ans: `C:\Users\simon.stark\Downloads\Stage-20240213T093324Z-001\Stage\invoice\invoices\invoice.bat`

# Task 4: Analyze the $Created0x30 timestamp for the previously identified file. When was this file created on disk?

> The $Created0x30 column represents the creation timestamp stored in the $FILE_NAME (0x30) attribute.

From the $Created0x30 column, we can find the exact timestamp. (Previous screenshot)

Ans: `2024-02-13 16:38:39`

# Task 5: Finding the hex offset of an MFT record is beneficial in many investigative scenarios. Find the hex offset of the stager file from Question 3.

> Hex Offset = MFT Entry Number × 1024

The entry number of the `invoice.bat` file is `23436`, so `23436 × 1024 = 23,998,464`.

We convert this number to hex and get: 

<img width="496" height="436" alt="image" src="https://github.com/user-attachments/assets/05f380e1-6189-411f-9a78-ab14dc5857d7" />

Ans: `16E3000`

# Task 6: Each MFT record is 1024 bytes in size. If a file on disk has smaller size than 1024 bytes, they can be stored directly on MFT File itself. These are called MFT Resident files. During Windows File system Investigation, its crucial to look for any malicious/suspicious files that may be resident in MFT. This way we can find contents of malicious files/scripts. Find the contents of The malicious stager identified in Question3 and answer with the C2 IP and port.

> HxD is a free, fast hexadecimal editor, disk editor, and memory editor developed by Maël Hörz for Windows

In HxD, go to **Search > Go to** (or CTRL + G) and enter the hexadecimal offset of the MFT entry we identified earlier. Since the file is small, its `$DATA` attribute may be resident within the MFT record. In this case, the file is resident, allowing us to recover and inspect its contents directly from the `$MFT`.

<img width="246" height="252" alt="image" src="https://github.com/user-attachments/assets/deee6db1-f3fd-4306-baf1-6e7f745e6dc1" />

<img width="629" height="795" alt="image" src="https://github.com/user-attachments/assets/18666aef-83b0-492d-9ce3-4b65097c5687" />

At this offset, the MFT record begins with the FILE signature. By examining the resident $DATA attribute within the record, we can recover the contents of invoice.bat. The script reveals a connection to the C2 server at 43.204.110.203 over port 6666.

Ans: `43.204.110.203:6666`

# Conclusion

The BFT Sherlock provided hands-on experience with NTFS Master File Table (MFT) forensics and demonstrated how valuable filesystem metadata can be during an incident investigation. Using MFTECmd and Timeline Explorer, we identified the malicious files, analyzed timestamps, examined Zone.Identifier metadata, and traced the origin of the downloaded ZIP file.

We then used the MFT entry number to calculate the raw offset of the malicious invoice.bat file and inspected its record using HxD. Since its $DATA attribute was resident, we were able to recover the script directly from the `$MFT` and identify the C2 address `43.204.110.203:6666`.

This investigation highlights how the $MFT can provide both important filesystem metadata and, in the case of resident files, actual file contents that can reveal valuable Indicators of Compromise (IOCs).

<img width="568" height="282" alt="image" src="https://github.com/user-attachments/assets/cbe631e8-cde2-496f-9c45-7f9727898b35" />

