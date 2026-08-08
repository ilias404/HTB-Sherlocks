# MangoBleed
![mangobleed.png](/MangoBleedHTB/screenshots/mangobleed.png)

# Sherlock Scenario
> You were contacted early this morning to handle a high‑priority incident involving a suspected compromised server. The host, mongodbsync, is a secondary MongoDB server. According to the administrator, it's maintained once a month, and they recently became aware of a vulnerability referred to as MongoBleed. As a precaution, the administrator has provided you with root-level access to facilitate your investigation.
> You have already collected a triage acquisition from the server using UAC. Perform a rapid triage analysis of the collected artifacts to determine whether the system has been compromised, identify any attacker activity (initial access, persistence, privilege escalation, lateral movement, or data access/exfiltration), and summarize your findings with an initial incident assessment and recommended next steps.

# Task 1: What is the CVE ID designated to the MongoDB vulnerability explained in the scenario?

Google

Ans: `CVE-2025-14847`

# Task 2: What is the version of MongoDB installed on the server that the CVE exploited?

After extracting the provided artifacts, the MongoDB logs were examined by navigating to `\[root]\var\log\mongodb` and opening the `mongod.log` file.

![mongover.png](/MangoBleedHTB/screenshots/mongover.png)

Ans: `8.0.16`

# Task 3: Analyze the MongoDB logs to identify the attacker's remote IP address used to exploit the CVE.

Upon a preliminary review of the mongod.log file, a suspicious IP address was identified in the logs.

![mongoattip.png](/MangoBleedHTB/screenshots/mongoattip.png)

Ans: `65.0.76.43`

# Task 4: Based on the MongoDB logs, determine the exact date and time the attacker’s exploitation activity began (the earliest confirmed malicious event)

![date.png](/MangoBleedHTB/screenshots/date.png)


Ans: `2025-12-29 05:25:52`

