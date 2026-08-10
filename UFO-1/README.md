# UFO-1

<img width="1200" height="675" alt="image" src="https://github.com/user-attachments/assets/7abd0633-afb1-41b4-89cd-715a98a187f3" />

# Sherlock Scenario
> Being in the ICS Industry, your security team always needs to be up to date and should be aware of the threats targeting organizations in your industry. You just started as a Threat intelligence intern, with a bit of SOC experience. Your manager has given you a task to test your skills in research and how well can you utilize Mitre Att&ck to your advantage.
>
> Do your research on Sandworm Team, also known as BlackEnergy Group and APT44. Utilize Mitre ATT&CK to understand how to map adversary behavior and tactics in actionable form.
>
> Smash the assessment and impress your manager as Threat intelligence is your passion.

# Task 1: According to the sources cited by Mitre, in what year did the Sandworm Team begin operations?

For the next few tasks, we will use the [MITRE ATT&CK G0034 – Sandworm](https://attack.mitre.org/groups/G0034/) page as our primary reference to answer most of the questions.

<img width="940" height="348" alt="image" src="https://github.com/user-attachments/assets/679c3769-c347-4f9e-a294-6cb7a96dd741" />

Ans: `2009`

# Task 2: Mitre notes two credential access techniques used by the BlackEnergy group to access several hosts in the compromised network during a 2016 campaign against the Ukrainian electric power grid. One is LSASS Memory access (T1003.001). What is the Attack ID for the other?

Navigating to the [2016 Ukraine Electric Power Attack](https://attack.mitre.org/campaigns/C0025/) section on the Sandworm MITRE ATT&CK page, we can examine the **MITRE ATT&CK Navigator layer** associated with the campaign.


<img width="1536" height="638" alt="image" src="https://github.com/user-attachments/assets/552ff033-39e7-45cc-a7bf-c1130399eb4d" />


<img width="359" height="845" alt="image" src="https://github.com/user-attachments/assets/9f084b2c-966e-442b-88fa-b0ccfc760789" />

Ans: `T1110`

# Task 3: During the 2016 campaign, the adversary was observed using a VBS script during their operations. What is the name of the VBS file?

> `VBScript (Visual Basic Scripting Edition)` is a lightweight scripting language developed by Microsoft to automate tasks on Windows operating systems

By filtering the MITRE ATT&CK Sandworm page for "VBS" and reviewing the relevant activity, we can identify the name of the VBS file used by the adversary.

<img width="1415" height="142" alt="image" src="https://github.com/user-attachments/assets/7dc181db-c117-4714-bb10-246ea1ec23de" />

Ans: `ufn.vbs`

# Task 4: The APT conducted a major campaign in 2022. The server application was abused to maintain persistence. What is the Mitre Att&ck ID for the persistence technique was used by the group to allow them remote access?

Navigating to the [2022 Ukraine Electric Power Attack](https://attack.mitre.org/campaigns/C0034/) section on the Sandworm MITRE ATT&CK page, we can examine the **MITRE ATT&CK Navigator layer** associated with the campaign.


<img width="366" height="782" alt="image" src="https://github.com/user-attachments/assets/8567f3fe-afe3-461f-86f6-0e9428dbfc97" />

Ans: `T1505.003`

# Task 5: What is the name of the malware / tool used in question 4?

We can find the answer from the last screenshot.

Ans: `Neo-REGEORG`

# Task 6: Which SCADA application binary was abused by the group to achieve code execution on SCADA Systems in the same campaign in 2022?

<img width="1389" height="145" alt="image" src="https://github.com/user-attachments/assets/27010d4f-4ab7-42b1-a230-8a0226f43127" />

Ans: `scilc.exe`

# Task 7: Identify the full command line associated with the execution of the tool from question 6 to perform actions against substations in the SCADA environment.

We can find the answer from the last screenshot.

Ans: `C:\sc\prog\exec\scilc.exe -do pack\scil\s1.txt`

# Task 8: What malware/tool was used to carry out data destruction in a compromised environment during the same campaign?

<img width="1045" height="167" alt="image" src="https://github.com/user-attachments/assets/31cc3326-9218-4595-9f11-0a37ef2c727f" />

<img width="909" height="190" alt="image" src="https://github.com/user-attachments/assets/d4bfbb07-1adf-4410-b8c1-6e68c2a5dd8d" />

Ans: `CaddyWiper`

# Task 9: The malware/tool identified in question 8 also had additional capabilities. What is the Mitre Att&ck ID of the specific technique it could perform in Execution tactic?

<img width="265" height="477" alt="image" src="https://github.com/user-attachments/assets/490a2df8-af81-4f7a-918c-f8a3a8dafcbd" />

Ans: `T1106`

# Task 10: The Sandworm Team is known to use different tools in their campaigns. They are associated with an auto-spreading malware that acted as a ransomware while having worm-like features. What is the name of this malware?

If we read the description of Sandworm group description, we can actually find the answer.

<img width="925" height="332" alt="image" src="https://github.com/user-attachments/assets/3685689d-b1c2-43f9-8422-792d64d935e4" />

<img width="934" height="212" alt="image" src="https://github.com/user-attachments/assets/52eeadc6-285a-4c8b-913f-5a824d38598d" />

Ans: `NotPetya`

# Task 11: What was the Microsoft security bulletin ID for the vulnerability that the malware from question 10 used to spread around the world?

NotPetya is associated with the famous exploits known as **EternalBlue** and **EternalRomance**. By searching for their corresponding **Microsoft Security Bulletin IDs**, we can find the following:

<img width="685" height="695" alt="image" src="https://github.com/user-attachments/assets/6feae89b-e12d-4f23-ad73-1c9d7f95b2fc" />

Ans: `MS17-010`

# Task 12: What is the name of the malware/tool used by the group to target modems?

By filtering the page for the keyword **"modem"**, we find:

<img width="682" height="66" alt="image" src="https://github.com/user-attachments/assets/70c0fb1a-033e-447f-a324-39a0e8071e4e" />

Ans: `AcidRain`

# Task 13: Threat Actors also use non-standard ports across their infrastructure for Operational-Security purposes. On which port did the Sandworm team reportedly establish their SSH server for listening?

The answer can be found on the Sandworm MITRE ATT&CK group page under **Techniques Used**.

<img width="1190" height="85" alt="image" src="https://github.com/user-attachments/assets/c0102031-98ae-4c78-b92c-53722543ef51" />

Ans: `6789`

# Task 14: The Sandworm Team has been assisted by another APT group on various operations. Which specific group is known to have collaborated with them?





