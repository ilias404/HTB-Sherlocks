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

In legitimate cases, the DC IP address that we found earlier should respond to requests from `172.17.79.136` (Forela-WKstn002). However, it appears that another IP address is responding instead, which is rogue.

Ans: `172.17.79.135`
