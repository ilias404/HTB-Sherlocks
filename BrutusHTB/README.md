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

## wtmp, utmp and btmp Background

wtmp: The wtmp file records all historical login and logout events on a Linux system. It tracks who logged in, from which terminal or IP, at what time, and for how long. This file is useful for reconstructing past user sessions and analyzing system access over time.

utmp: The utmp file keeps track of currently logged-in users. It shows active sessions on the system, including the username, terminal, and login time. Commands like who or w read this file to display live user activity.

btmp: The btmp file logs failed login attempts, making it an important source for detecting brute-force attacks or unauthorized access attempts. Using tools like lastb or a parsing script, you can see which usernames and IPs attempted unsuccessful logins.
