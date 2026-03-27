# Noxious HTB
![noxious.jpg](/NoxiousHTB/screenshots/noxious.jpg)

# Sherlock Scenario
> The IDS device alerted us to a possible rogue device in the internal Active Directory network. The Intrusion Detection System also indicated signs of LLMNR traffic, which is unusual. It is suspected that an LLMNR poisoning attack occurred. The LLMNR traffic was directed towards Forela-WKstn002, which has the IP address 172.17.79.136. A limited packet capture from the surrounding time is provided to you, our Network Forensics expert. Since this occurred in the Active Directory VLAN, it is suggested that we perform network threat hunting with the Active Directory attack vector in mind, specifically focusing on LLMNR poisoning.

# Task 1: Its suspected by the security team that there was a rogue device in Forela's internal network running responder tool to perform an LLMNR Poisoning attack. Please find the malicious IP Address of the machine.

> LLMNR (Link-Local Multicast Name Resolution) is a Windows protocol used to resolve hostnames on a local network when DNS fails. When a device cannot determine the IP address of a hostname through DNS, it sends a multicast request asking other devices on the network if they know the address. Any device can respond to this request.

> LLMNR poisoning is an attack that exploits this behavior. An attacker on the same network listens for LLMNR requests and responds to them by pretending to be the requested host. When the victim device connects to the attacker, it attempts to authenticate, sending an NTLMv2 response. The attacker can capture this response and use it for password cracking or relay attacks.

We know the rogue device is inside the Active Directory network. To identify its IP, we first determine the Domain Controller’s IP. Legitimate LLMNR responses from the DC are normal, but any other machine responding to LLMNR requests from clients, especially by impersonating the DC or other servers, is suspicious.

Let's open the provided PCAP file using Wireshark and filter for DNS. 

> We can also filter for other protocols like LDAP and Kerberos, since the Domain Controller uses these protocols to provide its services.

![dns.png](/NoxiousHTB/screenshots/dns.png)

The DC IP address is `172.17.79.4`.

Let's filter for LLMNR now and see what we can find.

![llmnr.png](/NoxiousHTB/screenshots/llmnr.png)

In legitimate cases, the DC IP address that we found earlier should be the one to respond to requests from `172.17.79.136` (Forela-WKstn002). However, it appears that another IP address is responding instead, which is suspicious.

Ans: `172.17.79.135`

# Task 2: What is the hostname of the rogue machine?

Now that we have the IP address, we can search for DHCP traffic related to it, which will reveal its hostname.

![kali.png](/NoxiousHTB/screenshots/kali.png)

Ans: `kali`

# Task 3: Now we need to confirm whether the attacker captured the user's hash and it is crackable!! What is the username whose hash was captured?

Filtering for SMB2 or NTLMSSP reveals the following:

![johndeacon.png](/NoxiousHTB/screenshots/johndeacon.png)

Ans: `john.deacon`

# Task 4: In NTLM traffic we can see that the victim credentials were relayed multiple times to the attacker's machine. When were the hashes captured the First time?

Selecting `View > Time Display Format > UTC Date and Time of Day` allows us to find the exact time and date when the hashes were first captured.

![timestamp.png](/NoxiousHTB/screenshots/timestamp.png)

> NTLMSSP authentication, as seen in network analysis tools like Wireshark, consists of three main messages exchanged between a client and a server. The process starts with the client sending an NTLMSSP_NEGOTIATE message, where it indicates that it wants to authenticate using NTLM and lists its supported security options. The server then responds with an NTLMSSP_CHALLENGE message, which contains a random value known as the server challenge. This value is used to ensure that the authentication process is unique and cannot be reused. Finally, the client sends an NTLMSSP_AUTH message, which includes the username, domain, and an NTLMv2 response computed using the user’s password hash and the server challenge. The server verifies this response, and if it matches the expected value, the authentication is successful; otherwise, it is rejected.

Ans: `2024-06-24 11:18:30`

# Task 5: What was the typo made by the victim when navigating to the file share that caused his credentials to be leaked?

![dcc01.png](/NoxiousHTB/screenshots/dcc01.png)


In the LLMNR traffic, we observed that the victim attempted to access `DCC01` instead of `DC01`, causing the DNS resolution to fail. As a result, the system fell back to LLMNR for name resolution. The attacker exploited this behavior by responding to the LLMNR request, impersonating the requested host (`DCC01`), and successfully capturing the victim’s authentication attempt through an LLMNR poisoning attack.

Ans: `DCC01`

# Task 6: To get the actual credentials of the victim user we need to stitch together multiple values from the ntlm negotiation packets. What is the NTLM server challenge value?

The NTLM server challenge value can be found under `SMB2 (Server Message Block Protocol version 2)` > `Session Setup Response (0x01)` > `Security Blob` -> `GSS-API Generic...` -> `Simple Protected Negotiation` > `negTokenTarg` > `NTLM Secure Service Provider` > `NTLM Server Challenge`.

![ntlmchall.png](/NoxiousHTB/screenshots/ntlmchall.png)

Ans: `601019d191f054f1`

# Task 7: Now doing something similar find the NTProofStr value.

![ntproof.png](/NoxiousHTB/screenshots/ntproof.png)

Ans: `c0cc803a6d9fb5a9082253a04dbd4cd4`

# Task 8: To test the password complexity, try recovering the password from the information found from packet capture. This is a crucial step as this way we can find whether the attacker was able to crack this and how quickly.

> To crack an NTLMv2 hash, several fields must be extracted because the authentication response is not a simple hash but the result of a computation that depends on multiple inputs. These required fields are the `username`, `domain`, `server challenge`, and the `NTLMv2 response`, which itself is composed of the `NTProofStr` and the `blob`.
>
> The `username` and `domain` are important because they are used in the generation of the NTLM hash and influence the final computed response. The `server challenge` is a random value sent by the server to ensure that each authentication attempt is unique. The `NTProofStr` represents the actual proof of knowledge of the password, while the `blob` contains additional data such as a timestamp and client-specific information.
> 
> During the cracking process, a tool like `Hashcat` takes a password guess and uses all these fields to recompute the NTLMv2 response. If the recomputed `NTProofStr` matches the captured one, the password is correct. Without any of these fields, the computation cannot be reproduced, making it impossible to verify password guesses.

The fields are combined into the following format:

`username::domain:server_challenge:NTProofStr:blob`

If we combine everything correctly, we should end up with something like this:

`john.deacon::FORELA:601019d191f054f1:c0cc803a6d9fb5a9082253a04dbd4cd4:010100000000000080e4d59406c6da01cc3dcfc0de9b5f2600000000020008004e0042004600590001001e00570049004e002d00360036004100530035004c003100470052005700540004003400570049004e002d00360036004100530035004c00310047005200570054002e004e004200460059002e004c004f00430041004c00030014004e004200460059002e004c004f00430041004c00050014004e004200460059002e004c004f00430041004c000700080080e4d59406c6da0106000400020000000800300030000000000000000000000000200000eb2ecbc5200a40b89ad5831abf821f4f20a2c7f352283a35600377e1f294f1c90a001000000000000000000000000000000000000900140063006900660073002f00440043004300300031000000000000000000`

We can save this to a file named `hash` and crack it later.

```bash
echo "john.deacon::FORELA:601019d191f054f1:c0cc803a6d9fb5a9082253a04dbd4cd4:010100000000000080e4d59406c6da01cc3dcfc0de9b5f2600000000020008004e0042004600590001001e00570049004e002d00360036004100530035004c003100470052005700540004003400570049004e002d00360036004100530035004c00310047005200570054002e004e004200460059002e004c004f00430041004c00030014004e004200460059002e004c004f00430041004c00050014004e004200460059002e004c004f00430041004c000700080080e4d59406c6da0106000400020000000800300030000000000000000000000000200000eb2ecbc5200a40b89ad5831abf821f4f20a2c7f352283a35600377e1f294f1c90a001000000000000000000000000000000000000900140063006900660073002f00440043004300300031000000000000000000" > hash
```

Let's try cracking it now using Hashcat:
```bash
hashcat -a 0 -m 5600 hash /home/worldline/Desktop/rockyou.txt
```

![cracked.png](/NoxiousHTB/screenshots/cracked.png)

Ans: `NotMyPassword0K?`

# Task 9: Just to get more context surrounding the incident, what is the actual file share that the victim was trying to navigate to?

By filtering for `smb2` and scrolling down a bit, we can find a some Tree Connect Requests and Responses.

> In SMB2 (Server Message Block v2), a Tree Connect refers to the process where a client connects to a specific shared resource on a server, such as a shared folder or printer. A Tree Disconnect is the opposite operation, where the client terminates that connection.

![confidential.png](/NoxiousHTB/screenshots/confidential.png)

Ans: `\\DC01\DC-Confidential`
