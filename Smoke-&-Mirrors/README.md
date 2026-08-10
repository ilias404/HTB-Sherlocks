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

Answer: `HKLM\SYSTEM\CurrentControlSet\Control\LSA`

# Task 2: Which PowerShell command did the attacker first execute to disable Windows Defender?

> `Set-MpPreference` is a PowerShell cmdlet used to modify Microsoft Defender Antivirus configuration and security preferences.

It is commonly encountered in security investigations because an attacker can abuse it to weaken or disable Defender's protective features.

To identify the first PowerShell command used by the attacker to disable Windows Defender, we filtered the logs by the relevant date and time and focused on Event ID 4104.

Among the resulting events, we identified the following command:

<img width="1289" height="229" alt="image" src="https://github.com/user-attachments/assets/4e0a0f96-18e2-4f87-a59d-e2e2b0debc6f" />

This command disables multiple Microsoft Defender security features, including real-time protection, script scanning, behavior monitoring, downloaded-file scanning, and intrusion prevention.

Answer: `Set-MpPreference -DisableIOAVProtection $true -DisableEmailScanning $true -DisableBlockAtFirstSeen $true`

# Task 3: The attacker loaded an AMSI patch written in PowerShell. Which function in the DLL is being patched by the script to effectively disable AMSI?



