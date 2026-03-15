# Brutus HTB

# Sherlock Description
> In this very easy Sherlock, you will familiarize yourself with Unix auth.log and wtmp logs. We'll explore a scenario where a Confluence server was brute-forced via its SSH service. After gaining access to the server, the attacker performed additional activities, which we can track using auth.log. Although auth.log is primarily used for brute-force analysis, we will delve into the full potential of this artifact in our investigation, including aspects of privilege escalation, persistence, and even some visibility into command execution.

# Task 1 : Analyze the auth.log. What is the IP address used by the attacker to carry out a brute force attack?

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

![catauth.png](/screenshots/catauth.png)
