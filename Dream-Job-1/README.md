# Dream Job-1

<img width="629" height="250" alt="image" src="https://github.com/user-attachments/assets/d972f188-c0e0-4eb4-9689-9de5df5f2f67" />

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

Under the Lateral Movement section of the MITRE ATT&CK Navigator layer:

<img width="163" height="626" alt="image" src="https://github.com/user-attachments/assets/cccf1ac9-ad39-416d-8907-30f6f6e824e8" />

Ans: `Internal Spearphishing`


# Task 6: What is the technique ID for the previous answer?

<img width="431" height="189" alt="image" src="https://github.com/user-attachments/assets/29f84920-1a11-402b-b666-b31f5799be8e" />

Ans: `T1534`


# Task 7: What Remote Access Trojan did the Lazarus Group use in Operation Dream Job?

Under the Software section of the MITRE ATT&CK page:

<img width="1390" height="188" alt="image" src="https://github.com/user-attachments/assets/4f2c37d3-c026-4fda-8cbf-56dc8ec3bb60" />

Ans: `DRATzarus`


# Task 8: What technique did the malware use for execution?

Under the Execution section:

<img width="159" height="714" alt="image" src="https://github.com/user-attachments/assets/0d05051d-001c-4f87-9077-ca2b7e40cca2" />

Ans: `Native API`


# Task 9: What technique did the malware use to avoid detection in a sandbox?

Under the Discovery section:

<img width="413" height="354" alt="image" src="https://github.com/user-attachments/assets/59cf7fcb-a0ce-4a28-81f7-a703a04f6ecd" />

Ans: `Time Based Checks`


# Task 10: To answer the remaining questions, utilize VirusTotal and refer to the IOCs.txt file. What is the name associated with the first hash provided in the IOC file?

<img width="743" height="274" alt="image" src="https://github.com/user-attachments/assets/66b04c05-43fa-4279-86e3-ee007408103c" />

Ans: `IEXPLORE.exe`

# Task 11: When was the file associated with the second hash in the IOC first created?

<img width="624" height="262" alt="image" src="https://github.com/user-attachments/assets/643391ca-7abe-42f7-a991-9a48e7753165" />

Ans: `2020-05-12 19:26:17`


# Task 12: What is the name of the parent execution file associated with the second hash in the IOC?

<img width="619" height="246" alt="image" src="https://github.com/user-attachments/assets/4ccfd5df-6842-4e8f-abd4-79d499cf3380" />

Ans: `BAE_HPC_SE.iso`


# Task 13: Examine the third hash provided. What is the file name likely used in the campaign that aligns with the adversary's known tactics?

<img width="736" height="267" alt="image" src="https://github.com/user-attachments/assets/ed194435-63f4-4845-b155-a83f82077b4c" />

Ans: `Salary_Lockheed_Martin_job_opportunities_confidential.doc`

# Task 14: Which malicious URL in the contacted URLs is used to fetch a secondary .docx file?

<img width="873" height="307" alt="image" src="https://github.com/user-attachments/assets/4755a8ab-72ae-4417-b98b-0b37c5dba158" />

Ans: `https://markettrendingcenter.com/lk_job_oppor.docx`

# Conclusion

This investigation provided an overview of the Operation Dream Job cyber-espionage campaign and the techniques and infrastructure associated with it. Using MITRE ATT&CK, we identified the responsible threat actor, associated campaigns, execution and lateral movement techniques, and the malware used by the Lazarus Group. The investigation was then extended using VirusTotal and the provided IOCs to correlate file hashes, filenames, execution artifacts, and malicious URLs.

The analysis demonstrates how threat intelligence platforms and frameworks such as MITRE ATT&CK and VirusTotal can be combined to attribute adversary activity, map TTPs, correlate IOCs, and reconstruct elements of a cyber-espionage campaign.

<img width="616" height="306" alt="image" src="https://github.com/user-attachments/assets/6602a3f4-ac44-4b34-9348-8f69a3f7beae" />
