# Reaper

<img width="1200" height="675" alt="image" src="https://github.com/user-attachments/assets/f8eb5772-241e-4ad8-aff8-598fb7e61ae7" />

# Sherlock Scenario
> Our SIEM alerted us to a suspicious logon event which needs to be looked at immediately . The alert details were that the IP Address and the Source Workstation name were a mismatch .You are provided a network capture and event logs from the surrounding time around the incident timeframe. Corelate the given evidence and report back to your SOC Manager.

# Task 1: What is the IP Address for Forela-Wkstn001?

By examining the first packet in Wireshark, we can identify the IP address assigned to Forela-Wkstn001.

![ip.png](/Reaper/screenshots/ip.png)

Ans: `172.17.79.129`

# Task 2: What is the IP Address for Forela-Wkstn002?

We can identify the IP address of Forela-Wkstn002 by navigating to `Edit > Find Packet`, setting the search type to `String`, and searching for the workstation's hostname.
Alternatively, we can filter for the `NBNS (NetBIOS Name Service)` protocol.
> `NBNS (NetBIOS Name Service)` is a protocol used by Windows systems to resolve NetBIOS hostnames to IP addresses on a local network. By examining the relevant NBNS packets, we can determine the IP address associated with Forela-Wkstn002.

![ip2.png](/Reaper/screenshots/ip2.png)

Ans: `172.17.79.136`

# Task 3: What is the username of the account whose hash was stolen by attacker?

By filtering the traffic for SMB2 and searching for NTLMSSP authentication messages, we can identify that the suspicious IP address `172.17.79.135` (hostname: `D`) is attempting NTLMSSP authentication using the account `arthur.kyle`.

![stolenhashuser.png](/Reaper/screenshots/stolenhashuser.png)

Ans: `arthur.kyle`

# Task 4: What is the IP Address of Unknown Device used by the attacker to intercept credentials?

The IP address of the unknown device was identified in the previous task.

Ans: `172.17.79.135`

# Task 5: What was the fileshare navigated by the victim user account?

By analyzing the `SMB2 Tree Connect Request` packets, we can identify that the victim user accessed the file share `\\DC01\Trip`.

![atriphuh.png](/Reaper/screenshots/atriphuh.png)

Ans: `\\DC01\Trip`

# Task 6: What is the source port used to logon to target workstation using the compromised account?

By analyzing the provided `Security.evtx` file, we can obtain additional details about the logon activity, including the source port used to authenticate to the target workstation with the compromised account.

![port.png](/Reaper/screenshots/port.png)

Ans: `40252`

# Task 7: What is the Logon ID for the malicious session?

In the same event, we can find the `Logon ID`

![logonid.png](/Reaper/screenshots/logonid.png)

Ans: `0x64a799`

# Task 8: The detection was based on the mismatch of hostname and the assigned IP Address. What is the workstation name and the source IP Address from which the malicious logon occur?

This information can be obtained from the Network Information section of the relevant event by viewing its details.

<img width="399" height="143" alt="image" src="https://github.com/user-attachments/assets/69d76064-fb4b-4836-af36-5d11e7889d5e" />

Ans: `FORELA-WKSTN002, 172.17.79.135`

# Task 9: At what UTC time did the the malicious logon happen?

We can find the exact UTC time under `System` details.

<img width="513" height="228" alt="image" src="https://github.com/user-attachments/assets/23f16eb8-c2a4-4216-8767-f7f58432d12c" />

Ans: `2024-07-31 04:55:16`

# Task 10: What is the share Name accessed as part of the authentication process by the malicious tool used by the attacker?

By examining Event ID 5140, we can identify the share name accessed by the malicious tool.

<img width="1105" height="500" alt="image" src="https://github.com/user-attachments/assets/0b95b48d-c902-4bea-9dec-67a8e2e030b8" />

Ans: `\\*\IPC$`

# Conclusion

The investigation revealed that the suspicious logon originated from the unknown device `172.17.79.135`, identified by the hostname `D`, rather than the legitimate workstation associated with the compromised account. The attacker used `NTLMSSP authentication` with the compromised account `arthur.kyle` indicating that the account's authentication credentials had been intercepted.

Further analysis of the network capture and Windows event logs showed that the malicious session originated from `172.17.79.135` while identifying itself as `FORELA-WKSTN002` which explains the hostname and IP address mismatch that triggered the SIEM alert.

The investigation also identified the victim's access to the `\\DC01\Trip` file share and the malicious authentication activity involving the `\\*\IPC$` share. By correlating the network traffic with the Security event logs, we were able to reconstruct the malicious logon activity and identify the source of the suspicious authentication.

<img width="668" height="448" alt="image" src="https://github.com/user-attachments/assets/ed8a15f8-91c9-4317-8c35-b8a9028d6980" />

