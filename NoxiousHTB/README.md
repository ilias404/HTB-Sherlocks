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

