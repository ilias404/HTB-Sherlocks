# Operation Blackout 2025: Phantom Check

# Sherlock Scenario
> Talion suspects that the threat actor carried out anti-virtualization checks to avoid detection in sandboxed environments. Your task is to analyze the event logs and identify the specific techniques used for virtualization detection. Byte Doctor requires evidence of the registry checks or processes the attacker executed to perform these checks.

# Task 1: Which WMI class did the attacker use to retrieve model and manufacturer information for virtualization detection?

Upon extracting the artifacts, we obtain two `.evtx` files to investigate. We begin with `Windows-PowerShell-Operational.evtx`.
One commonly used PowerShell cmdlet for retrieving information from WMI classes is `Get-WmiObject`. By filtering the PowerShell events for this cmdlet, we identify the following activity:

<img width="1310" height="270" alt="image" src="https://github.com/user-attachments/assets/0764d86a-d8ae-41ee-999f-fa231eb3b19c" />

Ans: `Win32_ComputerSystem`

# Task 2: Which WMI query did the attacker execute to retrieve the current temperature value of the machine?

Using the same approach, we identify the WMI query used by the attacker to retrieve the current temperature value of the machine

<img width="1337" height="206" alt="image" src="https://github.com/user-attachments/assets/0452bc6e-00ce-487f-b9b3-67aa07735f3d" />

Ans: `SELECT * FROM MSAcpi_ThermalZoneTemperature`

# Task 3: The attacker loaded a PowerShell script to detect virtualization. What is the function name of the script?

After filtering the logs to display only **Event ID 4104** events and reviewing the resulting PowerShell Scripts, we came across the following:

<img width="549" height="516" alt="image" src="https://github.com/user-attachments/assets/521ff860-0882-4798-b622-e26108a3e1a8" />
<img width="1323" height="616" alt="image" src="https://github.com/user-attachments/assets/a77c2fe3-df7c-4e4e-b253-33ed131f8013" />

Ans: `Check-VM`

# Task 4: Which registry key did the above script query to retrieve service details for virtualization detection?
Reading through the PowerShell script, we find that the registry key `HKLM:\SYSTEM\ControlSet001\Services` is queried to retrieve service information.

<img width="1299" height="660" alt="image" src="https://github.com/user-attachments/assets/55650c52-ab05-4345-b28a-06b4f0bec6a4" />

Ans: `HKLM:\SYSTEM\ControlSet001\Services`
