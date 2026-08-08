# Operation Blackout 2025: Phantom Check

<img width="872" height="256" alt="image" src="https://github.com/user-attachments/assets/21034f0a-b563-4440-a437-d6a600956757" />

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

# Task 5: The VM detection script can also identify VirtualBox. Which processes is it comparing to determine if the system is running VirtualBox?

In the PowerShell script, we can find the process names to identify whether the machine is on VirtualBox or not.

<img width="431" height="290" alt="image" src="https://github.com/user-attachments/assets/994881d2-0ec9-465a-bca8-d86e15846100" />

Ans: `vboxservice.exe, vboxtray.exe`

# Task 6: The VM detection script prints any detection with the prefix 'This is a'. Which two virtualization platforms did the script detect?
To identify the virtualization platforms detected by the script, we search the PowerShell logs for the string `This is a`, which is used as the prefix for each detection. Reviewing the matching events reveals the two virtualization platforms identified by the VM detection script.

<img width="1314" height="313" alt="image" src="https://github.com/user-attachments/assets/08074c2a-1926-4634-9ad0-e33a06f4260a" />


Ans: `Hyper-V, Vmware`

# Conclusion

The investigation of the PowerShell operational logs revealed that the threat actor used multiple techniques to determine whether the system was running in a virtualized environment. The activity included querying the `Win32_ComputerSystem` WMI class for manufacturer and model information, retrieving thermal-zone information through `MSAcpi_ThermalZoneTemperature`, and executing the `Check-VM` PowerShell function.

Overall, the evidence demonstrates that the threat actor implemented several **anti-virtualization techniques** to fingerprint the environment and potentially avoid execution in sandboxed or analysis environments.

<img width="879" height="446" alt="image" src="https://github.com/user-attachments/assets/28586e5b-b42e-4031-8a07-f0792bec43bb" />




