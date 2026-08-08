# Operation Blackout 2025: Phantom Check

# Sherlock Scenario
> Talion suspects that the threat actor carried out anti-virtualization checks to avoid detection in sandboxed environments. Your task is to analyze the event logs and identify the specific techniques used for virtualization detection. Byte Doctor requires evidence of the registry checks or processes the attacker executed to perform these checks.

# Task 1: Which WMI class did the attacker use to retrieve model and manufacturer information for virtualization detection?

Upon extracting the artifacts, we obtain two `.evtx` files to investigate. We begin with `Windows-PowerShell-Operational.evtx`.
One commonly used PowerShell cmdlet for retrieving information from WMI classes is `Get-WmiObject`. By filtering the PowerShell events for this cmdlet, we identify the following activity:

<img width="1310" height="270" alt="image" src="https://github.com/user-attachments/assets/0764d86a-d8ae-41ee-999f-fa231eb3b19c" />

Ans: `Win32_ComputerSystem`
