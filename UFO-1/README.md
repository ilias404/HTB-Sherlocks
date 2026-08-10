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

Ans: `Neo-REGEORG`

# Task 5: Which SCADA application binary was abused by the group to achieve code execution on SCADA Systems in the same campaign in 2022?


