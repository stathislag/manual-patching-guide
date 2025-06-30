


# 🛠 The Ultimate Guide to Manual Patching on Windows & Linux

## 👨‍💻 What a Patcher Really Does

In manual environments, **you are the automation**. From update discovery to rollback, it's all in your hands. Your role ensures security, uptime, and operational continuity without scripts or tools doing the heavy lifting. And yes, **backups and logs are your best friends**.

---

## 🔁 Patch Lifecycle Awareness

Every OS version goes through:

- **Mainstream Support** → New features + bug fixes
- **Extended Support** → Only security patches
- **End-of-Life (EOL)** → No updates; consider ESUs (Windows) or OS upgrades

🔗 [https://endoflife.date](https://endoflife.date) – check support timelines for all major platforms

---

## ✅ How to Check Patch Status

### 🔷 Windows
- **GUI**: Settings → Windows Update → Check for updates  
- **CLI**:
  - `Get-HotFix` — Installed patches
  - `systeminfo` — Build version
  - `sconfig` — For Server Core
  - `Get-WindowsUpdateLog` — Review log output

### 🐧 Linux
- **Ubuntu/Debian**:
  - `apt update && apt list --upgradable`
  - `apt policy <package>`
- **RHEL/CentOS/Alma**:
  - `dnf check-update`
  - `rpm -qa --last`
- **SUSE**:
  - `zypper lu`
- **Arch**:
  - `pacman -Qu`

---

## ☑ How to Confirm Patch Success

### Windows
- Check: `Event Viewer > Windows Logs > Setup`
- CBS log: `C:\\Windows\\Logs\\CBS\\CBS.log`

### Linux
- Debian: `/var/log/dpkg.log`
- RHEL: `/var/log/yum.log`, `journalctl`
- SUSE: `zypper log`, `zypper ps`

---

## 🚨 What Is a Critical Patch?

Prioritize patches that:

- Fix zero-days or CVEs with **CVSS > 7**
- Prevent **remote code execution**
- Patch **kernel or privilege escalation** issues

---

## 🧩 Manual Patching – Windows

### Finding Updates
- **GUI**: Settings > Update & Security
- **PowerShell**:
  - `Get-WindowsUpdate -MicrosoftUpdate`
  - `usoclient StartScan` (older)
  - `wuauclt /detectnow` (very old)

### Installing Updates
- `.msu` files:  
  `wusa.exe C:\\Path\\update.msu /quiet /norestart`

- PowerShell:  
  `Install-WindowsUpdate -MicrosoftUpdate -AcceptAll -AutoReboot` *(requires PSWindowsUpdate)*

### Rollback
- `wusa.exe /uninstall /kb:XXXXXXX /quiet /norestart`

---

## 🧩 Manual Patching – Linux

### Ubuntu/Debian
```bash
sudo apt update && sudo apt upgrade
apt-mark hold <package>
sudo apt install <package>=<version>
```

### RHEL/CentOS/Alma
```bash
sudo dnf check-update
sudo dnf update
dnf history
dnf history undo <ID>
```

### SUSE
```bash
sudo zypper refresh
sudo zypper update
zypper al <package>
```

### Arch
```bash
sudo pacman -Syu
downgrade <package>
```

---

## 📉 When Patching Fails – What to Check

### Windows
- CBS.log, Event Viewer
- `sfc /scannow`, `DISM /Online /Cleanup-Image /RestoreHealth`
- Reset update services:
```cmd
net stop wuauserv && net stop bits
del /q C:\Windows\SoftwareDistribution\Download\*
net start wuauserv && net start bits
```

### Linux
- Debian:
```bash
sudo apt --fix-broken install
sudo dpkg --configure -a
```
- RHEL:
```bash
rm -f /var/lib/rpm/__db*
rpm --rebuilddb
```
- Arch:
```bash
sudo pacman-key --init
sudo pacman-key --populate archlinux
```

---

## 🔁 Recovery Methods

- **Windows**: Uninstall KBs, use restore points, or VSS
- **Linux**: Downgrade package, restore snapshots, reinstall from ISO or backup

---

## 📊 Trackable Patching KPIs

- ✅ Patch Success Rate
- ⏱ Mean Time to Patch (MTTP)
- 🕒 Time from Patch Release to Deployment
- 🧯 Incidents Post-Patch

---

## 🔥 Common Admin Mistakes

- No **snapshot/backup** before patching
- Skipping **release notes**
- Patching **directly in production**
- Forgetting to **verify/reboot**

---

## 🧘 The Patcher’s Mindset

Patching is a discipline.  
Plan. Validate. Patch. Verify.  
And **never patch production on a Friday. Ever.**
