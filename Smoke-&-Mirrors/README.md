# Smoke-&-Mirrors

# Sherlock Scenario
> Byte Doctor Reyes is investigating a stealthy post-breach attack where several expected security logs and Windows Defender alerts appear to be missing. He suspects the attacker employed defense evasion techniques to disable or manipulate security controls, significantly complicating detection efforts.
> Using the exported event logs, your objective is to uncover how the attacker compromised the system's defenses to remain undetected.

# Task 1: The attacker disabled LSA protection on the compromised host by modifying a registry key. What is the full path of that registry key?

The attacker disabled LSA protection on the compromised host by modifying a registry key.

While reviewing the logs, we identified the following command, which was executed to modify the Windows Registry:

<img width="1291" height="256" alt="image" src="https://github.com/user-attachments/assets/3e2ac72c-df15-4602-a917-6b6a39478e85" />

The command sets the `RunAsPPL` registry value to `0`, effectively disabling LSA Protection.

Answer: `HKLM\SYSTEM\CurrentControlSet\Control\LSA`

# Task 2: Which PowerShell command did the attacker first execute to disable Windows Defender?




