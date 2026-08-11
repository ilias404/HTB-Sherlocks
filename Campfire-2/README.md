# Campfire-2

# Sherlock Scenario

> Forela's Network is constantly under attack. The security system raised an alert about an old admin account requesting a ticket from KDC on a domain controller.
> Inventory shows that this user account is not used as of now so you are tasked to take a look at this. This may be an AsREP roasting attack as anyone can request any user's ticket which has preauthentication disabled.

# Task 1: When did the ASREP Roasting attack occur, and when did the attacker request the Kerberos ticket for the vulnerable user?
The Kerberos ticket request can be identified using **Event ID 4768**, which is generated when a Kerberos authentication service request for a Ticket Granting Ticket (TGT) is made.

Since AS-REP Roasting targets accounts with Kerberos pre-authentication disabled, we can filter the logs for **Event ID 4768** and look for events where the `Pre-Authentication Type` is set to `0`.

<img width="532" height="477" alt="image" src="https://github.com/user-attachments/assets/8fccea60-d0e1-4d1b-945c-0f0864413112" />

<img width="516" height="403" alt="image" src="https://github.com/user-attachments/assets/b41d7e87-6ed5-4cc1-b522-fa6042f013dd" />

By examining the event, we identified the vulnerable user, the source system that requested the ticket, and the exact timestamp of the request.

<img width="883" height="297" alt="image" src="https://github.com/user-attachments/assets/5bc2871e-c051-460d-a0d3-d0eb4ccb3e96" />

Ans: `2024-05-29 06:36:40`

# Task 2: Please confirm the User Account that was targeted by the attacker.

We can find it from the previous task.

Ans: `arthur.kyle`

# Task 3: What was the SID of the account?

> The `SID (Security Identifier)` is a unique identifier assigned to a Windows account, user, group, or computer.

In the same event, we can find the SID of the user's account.

<img width="676" height="303" alt="image" src="https://github.com/user-attachments/assets/c32ae6c1-0db9-4cca-abd4-b6b393cdc7c1" />

Ans: `S-1-5-21-3239415629-1862073780-2394361899-1601`

# Task 4: It is crucial to identify the compromised user account and the workstation responsible for this attack. Please list the internal IP address of the compromised asset to assist our threat-hunting team.

In the same event: 

<img width="580" height="411" alt="image" src="https://github.com/user-attachments/assets/1fc57ccb-2ea8-4a10-b792-8adf1cb69305" />

Ans: `172.17.79.129`

# Task 5: We do not have any artifacts from the source machine yet. Using the same DC Security logs, can you confirm the user account used to perform the ASREP Roasting attack so we can contain the compromised account/s?

After clearing the filters, we can examine events that occurred after the previously identified timestamp to identify any activity associated with the attack.

<img width="1285" height="473" alt="image" src="https://github.com/user-attachments/assets/3ac676ea-9b07-436f-a88d-c676f2cca90b" />

By reviewing the Event ID 4769 that occurs directly after the attack event, we can identify the user account used to perform the attack, along with other relevant details.

Ans: `happy.grunwald`

