# Campfire-1 HTB

![campfire1.jpg](/Campfire-1HTB/screenshots/campfire1.jpg)

# Sherlock Scenario
> Alonzo Spotted Weird files on his computer and informed the newly assembled SOC Team. Assessing the situation it is believed a Kerberoasting attack may have occurred in the network. It is your job to confirm the findings by analyzing the provided evidence.
>
>You are provided with:
>
>1- Security Logs from the Domain Controller
>
>2- PowerShell-Operational Logs from the affected workstation
>
>3- Prefetch Files from the affected workstation

# Task 1: Analyzing Domain Controller Security Logs, can you confirm the UTC date & time when the kerberoasting activity occurred?

To identify the UTC date and time of the Kerberoasting activity, let’s analyze the Domain Controller security logs provided by focusing on Event ID 4769.
> Event ID 4769 in Microsoft Windows records Kerberos service ticket requests. The Ticket Encryption Type indicates the encryption used: 0x17 (RC4) is weak and often associated with Kerberoasting, while 0x12 / 0x14 (AES) are secure and typically reflect normal activity.

![0x17.jpg](/Campfire-1HTB/screenshots/0x17.jpg)
![ts.jpg](/Campfire-1HTB/screenshots/ts.jpg)


