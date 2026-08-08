# Dream Job-1
# Sherlock Scenario
> You are a junior threat intelligence analyst at a Cybersecurity firm. You have been tasked with investigating a Cyber espionage campaign known as Operation Dream Job. The goal is to gather crucial information about this operation.

# Task 1: Who conducted Operation Dream Job?
For the next nine tasks, we will refer to [this](https://attack.mitre.org/campaigns/C0022/) MITRE ATT&CK page to identify and analyze the relevant information.

<img width="930" height="240" alt="image" src="https://github.com/user-attachments/assets/b5f7cf29-5eab-4d58-bfdf-6dd9cbfbd35d" />

Ans: `Lazarus Group`

# Task 2: When was this operation first observed?

<img width="392" height="266" alt="image" src="https://github.com/user-attachments/assets/1e063bfb-d020-4257-96b5-f3e3934d02eb" />


Ans: `September 2019`


# Task 3: There are 2 campaigns associated with Operation Dream Job. One is Operation North Star, what is the other?

The answer to this task can be identified from the screenshot provided in Task 1.

Ans: `Operation Interception`


# Task 4: During Operation Dream Job, there were the two system binaries used for proxy execution. One was Regsvr32, what was the other?

To identify the second system binary used for proxy execution, we can use the MITRE ATT&CK Navigator layer associated with Operation Dream Job.

<img width="404" height="152" alt="image" src="https://github.com/user-attachments/assets/1bf4ef5d-16f4-466a-99ae-867a6e3c2e78" />

<img width="513" height="568" alt="image" src="https://github.com/user-attachments/assets/af79f1e7-bcd0-46c5-a6e0-70409e2f3f98" />


Ans: `Rundll32`


# Task 5: What lateral movement technique did the adversary use?
Ans: `Internal Spearphishing`


# Task 6: What is the technique ID for the previous answer?
Ans: `T1534`


# Task 7: What Remote Access Trojan did the Lazarus Group use in Operation Dream Job?
Ans: `DRATzarus`


# Task 8: What technique did the malware use for execution?
Ans: `Native API`


# Task 9: What technique did the malware use to avoid detection in a sandbox?
Ans: `Time Based Checks`


# Task 10: To answer the remaining questions, utilize VirusTotal and refer to the IOCs.txt file. What is the name associated with the first hash provided in the IOC file?
Ans: `T1534`


# Task 11: When was the file associated with the second hash in the IOC first created?
Ans: `T1534`


# Task 12: What is the name of the parent execution file associated with the second hash in the IOC?
Ans: `T1534`


# Task 13: Examine the third hash provided. What is the file name likely used in the campaign that aligns with the adversary's known tactics?
Ans: `T1534`



# Task 14: Which malicious URL in the contacted URLs is used to fetch a secondary .docx file?
Ans: `T1534`


