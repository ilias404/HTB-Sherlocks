# Smoke-&-Mirrors

# Sherlock Scenario
> Byte Doctor Reyes is investigating a stealthy post-breach attack where several expected security logs and Windows Defender alerts appear to be missing. He suspects the attacker employed defense evasion techniques to disable or manipulate security controls, significantly complicating detection efforts.
> Using the exported event logs, your objective is to uncover how the attacker compromised the system's defenses to remain undetected.

# Task 1: The attacker disabled LSA protection on the compromised host by modifying a registry key. What is the full path of that registry key?

> LSA (Local Security Authority) is a Windows security subsystem responsible for enforcing local security policies and handling important authentication and authorization operations.

The attacker disabled LSA protection on the compromised host by modifying a registry key.

While reviewing the logs, we identified the following command, which was executed to modify the Windows Registry:

<img width="1291" height="256" alt="image" src="https://github.com/user-attachments/assets/3e2ac72c-df15-4602-a917-6b6a39478e85" />

The command sets the `RunAsPPL` registry value to `0`, effectively disabling LSA Protection.

Ans: `HKLM\SYSTEM\CurrentControlSet\Control\LSA`

# Task 2: Which PowerShell command did the attacker first execute to disable Windows Defender?

> `Set-MpPreference` is a PowerShell cmdlet used to modify Microsoft Defender Antivirus configuration and security preferences.
> It is commonly encountered in security investigations because an attacker can abuse it to weaken or disable Defender's protective features.

To identify the first PowerShell command used by the attacker to disable Windows Defender, we filtered the logs by the relevant date and time and focused on Event ID 4104.

Among the resulting events, we identified the following command:

<img width="1289" height="229" alt="image" src="https://github.com/user-attachments/assets/4e0a0f96-18e2-4f87-a59d-e2e2b0debc6f" />

This command disables multiple Microsoft Defender security features, including real-time protection, script scanning, behavior monitoring, downloaded-file scanning, and intrusion prevention.

Ans: `Set-MpPreference -DisableIOAVProtection $true -DisableEmailScanning $true -DisableBlockAtFirstSeen $true`

# Task 3: The attacker loaded an AMSI patch written in PowerShell. Which function in the DLL is being patched by the script to effectively disable AMSI?

> `Antimalware Scan Interface (AMSI)` is a versatile Windows standard that allows applications and services—such as PowerShell, VBScript, and Office macros—to integrate with installed antivirus products and scan dynamic or fileless scripts for malicious content just before execution.
>
> An `AMSI patch` is a modification to an AMSI component, typically performed in memory, that interferes with AMSI's normal scanning functionality. Attackers may use AMSI patches as a defense-evasion technique to prevent malicious PowerShell or other script content from being properly inspected by security software.

By analyzing the PowerShell Event ID 4104 and searching for `.dll`, we found that the script uses `GetProcAddress()` to locate a function in `amsi.dll`.

The concatenated string resolves to: AmsiScanBuffer

<img width="1278" height="419" alt="image" src="https://github.com/user-attachments/assets/7010e663-06a8-4941-bdcc-4139344ab8ee" />

This function is then modified in memory to interfere with AMSI scanning.

Ans: `AmsiScanBuffer`

# Task 4: Which command did the attacker use to restart the machine in Safe Mode?

By filtering PowerShell Event ID 4104, we identified the command used to configure the system for Safe Mode.

<img width="1297" height="254" alt="image" src="https://github.com/user-attachments/assets/5a42e280-5bb2-4671-aeea-0ce618ad481d" />

To determine the exact command used to restart the machine, we searched the provided `Microsoft-Windows-Sysmon/Operational` logs around the same timestamp, `07:38:35`.

<img width="1303" height="501" alt="image" src="https://github.com/user-attachments/assets/856ac733-b28d-4d0b-a158-bc09ce331432" />

Ans: `bcdedit.exe /set safeboot network`

# Task 5: Which PowerShell command did the attacker use to disable PowerShell command history logging?

In the Event ID 4104 events following the Safe Mode activity, we identified a PowerShell command used to disable command history logging:

<img width="1276" height="216" alt="image" src="https://github.com/user-attachments/assets/a5d18582-6cde-4e56-99a4-348fab9cd2d6" />

This prevents PowerShell command history from being saved to disk, reducing the forensic traces left by commands executed during the session.

Ans: `Set-PSReadlineOption -HistorySaveStyle SaveNothing`





