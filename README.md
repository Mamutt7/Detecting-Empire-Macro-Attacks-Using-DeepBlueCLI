# Detecting Empire/Starkiller windows_launcher_vbs attacks using DeepBlueCLI and Windowsevntvwr

## 1. Empire Setup on Ubuntu
![Empire Ready](https://github.com/user-attachments/assets/715bac52-ed66-4abf-8b9d-ebbf85d83253)
> **Figure 1.** Empire server running on Ubuntu, ready to generate and host stagers.

---

## 2. Listener Configuration
![Listener Setup](https://github.com/user-attachments/assets/6f97ef2e-ddda-4886-a075-b3a139702fda)
> **Figure 2.** Configuring an HTTP listener in Empire (Starkiller UI).

---

## 3. Stager Generation
![Stager Choices](https://github.com/user-attachments/assets/07e6c455-5c5d-4861-aea5-1037338b14fc)
> **Figure 3.** Available Windows stagers in Starkiller; we chose **windows_launcher_vbs**.

---

## 4. Payload Execution & Agent Check-in
![Agent C56V6Z34](https://github.com/user-attachments/assets/7c13aa9b-0bda-4c1c-9815-5918c7ee122e)
> **Figure 4.** After launching the VBS stager on the Windows10-VM, Empire agent **C56V6Z34** appears.

---

## 5. Situational Awareness (System Info)
![System Info](https://github.com/user-attachments/assets/6371bc2d-634d-4cfe-bb8d-2c1af1330037)
> **Figure 5.** Running `shell systeminfo` in the agent terminal to gather host details.

---

## 6. Task Manager Verification
![Task Manager](https://github.com/user-attachments/assets/483c3a95-e516-4061-b2fe-fa383e5ecb2e)
> **Figure 6.** Windows Task Manager confirming our stager process is running.

---

## 7. Manual Log Inspection (Event Viewer)
![Event Viewer](https://github.com/user-attachments/assets/428440ff-7bdd-49fc-a355-fe036a152df6)
> **Figure 7.** Searching **Windows Event Viewer** for evidence of stager execution under **Security** logs.

---

## 8. DeepBlueCLI Log Parsing Attempts
![DeepBlueCLI Error](https://github.com/user-attachments/assets/8f7e9495-6243-4a0a-8169-1d7ce7af48b7)
> **Figure 8.** Attempting to run **DeepBlueCLI** (`DeepBlue.ps1`) to parse recent Security events; encountered module/path issues.

---

## Summary & Next Steps

1. **Successes (Red Team):**  
   - Generated and deployed a **windows_launcher_vbs** stager from Starkiller.  
   - Achieved a working Empire agent (`C56V6Z34`) on a vanilla Windows VM.  
   - Collected host information via `systeminfo` and verified process execution.

2. **Challenges (Blue Team):**  
   - The **Security** event log did not show clear new entries attributed to our stager (likely because VBS runs under PowerShell host).  
   - **DeepBlueCLI** integration issues—script path/module resolution—prevented automated event parsing.

3. **Recommendations:**  
   - For richer audit trails, consider stagers that trigger **Security** events directly (e.g., new service, registry modifications).  
   - Ensure **DeepBlueCLI** prerequisites: correct working directory, PowerShell execution policy, module import path.  
   - Supplement with **PowerShell operational** and **Application** logs when hunting for script-based attacks.

---

*Copy this `README.md` into your GitHub repo. It contains all screenshots, captions, and analysis for a 5–10 min walkthrough of the red-blue exercise.*
