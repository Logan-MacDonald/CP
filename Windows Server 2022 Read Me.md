# Windows Server 2022 Security Hardening Script

A comprehensive PowerShell security audit and hardening script for Windows Server 2022, based on industry standards including CIS Benchmarks, Microsoft Security Baselines, and NIST guidelines.

## 🛡️ Features

This script automates security checks and hardening across 17 key areas:

| # | Area | Description |
|---|------|-------------|
| 1 | System Information | OS version, domain status, prerequisites |
| 2 | Account Policies | Password policy, lockout policy (CIS 1.1-1.2) |
| 3 | User Rights | Debug programs, remote access, shutdown rights (CIS 2.2) |
| 4 | Security Options | UAC, interactive logon, network security (CIS 2.3) |
| 5 | Windows Firewall | Profile status, logging, rules audit (CIS 9) |
| 6 | Audit Policy | Advanced audit policies, event log sizing (CIS 17) |
| 7 | **Hacking Tools** | **Detect & remove security/hacking tools** |
| 8 | Windows Services | Disable dangerous services, verify essential services (CIS 5) |
| 9 | Windows Defender | Real-time protection, behavior monitoring, signatures |
| 10 | SMB Configuration | SMBv1 disable, signing, encryption |
| 11 | Remote Desktop | NLA, encryption, session timeouts (CIS 18.9.65) |
| 12 | TLS/SSL | Disable SSL 2.0/3.0, TLS 1.0/1.1; Enable TLS 1.2/1.3 |
| 13 | Windows Update | Auto-update status, pending updates, WSUS |
| 14 | Additional Security | PowerShell logging, AutoRun, LSA protection, WDigest |
| 15 | IIS Security | App pool identities, HTTPS, directory browsing |
| 16 | Users & Groups | Account audit, admin group, password policies |
| 17 | Network Config | Adapters, DNS, ports, IPv6, LLMNR |

## 🔍 Hacking Tools Detection

The script scans for and offers to remove potentially dangerous security/hacking tools:

### Categories Detected

| Category | Tools |
|----------|-------|
| **Network Scanning** | Nmap, Zenmap, Masscan, Angry IP Scanner, Nessus, OpenVAS |
| **Password Cracking** | John the Ripper, Hashcat, Ophcrack, Hydra, Medusa, L0phtCrack, Cain & Abel |
| **Exploitation** | Metasploit, Cobalt Strike, Empire, BeEF, SET, Veil Framework |
| **Network Sniffing** | Wireshark, Ettercap, Responder, Bettercap, NetworkMiner |
| **Wireless** | Aircrack-ng, Kismet, Wifite, Fern WiFi Cracker |
| **Web Testing** | Burp Suite, OWASP ZAP, Nikto, SQLMap, DirBuster |
| **Credential Theft** | Mimikatz, LaZagne, Windows Credentials Editor, Rubeus |
| **Remote Access** | Netcat, PuTTY (flagged), TeamViewer (flagged), AnyDesk (flagged) |
| **Recon Tools** | Maltego, theHarvester, Recon-ng, SpiderFoot, BloodHound |
| **PowerShell** | PowerSploit, Nishang, PowerView, PowerUp, Invoke-Mimikatz |

### Detection Methods

1. **Registry Scan** - Checks installed programs in Windows Registry
2. **File System Scan** - Searches common directories for tool executables
3. **Windows Features** - Checks for Telnet Client, TFTP, SMBv1
4. **PowerShell Modules** - Scans for offensive PowerShell modules
5. **Running Processes** - Identifies suspicious running processes
6. **Python Packages** - Checks pip for security tools (if Python installed)
7. **Suspicious Directories** - Looks for folders like C:\Tools, C:\Hacking, C:\Pentest

## 📋 Prerequisites

- Windows Server 2022 (also compatible with 2019)
- PowerShell 5.1 or higher
- Administrator privileges
- ~50MB disk space for logs and backups

## 🚀 Installation

```powershell
# Download the script to your server
# No installation required - just run the script
```

## 📖 Usage

### Check-Only Mode (Safe Audit)
```powershell
.\Windows_Server_2022_Hardening.ps1 -Mode CheckOnly
```
Performs all security checks without making any changes. Ideal for initial assessment.

### Interactive Mode (Default)
```powershell
.\Windows_Server_2022_Hardening.ps1 -Mode Interactive
```
Prompts for confirmation before each change. Recommended for most users.

### Auto-Fix Mode
```powershell
.\Windows_Server_2022_Hardening.ps1 -Mode AutoFix
```
Automatically applies all security fixes. Use with caution in production.

### Generate HTML Report
```powershell
.\Windows_Server_2022_Hardening.ps1 -Mode CheckOnly -GenerateReport
```
Creates a detailed HTML report in addition to console output.

## 📊 Output

The script generates:

1. **Console Output** - Color-coded results
   - 🟢 Green: Compliant/Fixed
   - 🔴 Red: Issues found
   - 🟡 Yellow: Warnings
   - 🔵 Blue: Information

2. **Log File**: `C:\SecurityHardening\Logs\Hardening_YYYYMMDD_HHMMSS.log`

3. **Backup Directory**: `C:\SecurityHardening\Backups\YYYYMMDD_HHMMSS\`
   - Registry exports before changes
   - Security policy exports

4. **HTML Report** (optional): `C:\SecurityHardening\Reports\Report_YYYYMMDD_HHMMSS.html`

## 🔧 Key Configurations Applied

### Password Policy (CIS 1.1)
| Setting | Value |
|---------|-------|
| Password History | 24 passwords |
| Maximum Age | 90 days |
| Minimum Age | 1 day |
| Minimum Length | 14 characters |

### Account Lockout (CIS 1.2)
| Setting | Value |
|---------|-------|
| Lockout Duration | 30 minutes |
| Lockout Threshold | 5 attempts |
| Reset Counter | 30 minutes |

### Network Security
| Setting | Value |
|---------|-------|
| LAN Manager Auth | NTLMv2 only |
| SMBv1 | Disabled |
| SMB Signing | Required |
| TLS 1.0/1.1 | Disabled |
| TLS 1.2/1.3 | Enabled |

### Services Disabled
- Remote Registry
- Telnet
- FTP Publishing
- SNMP (if not needed)
- Xbox services
- Windows Subsystem for Linux
- UPnP Device Host

## ⚠️ Important Considerations

### Before Running
1. **Backup your system** - Create a full system backup or snapshot
2. **Test in non-production** - Always test changes in a lab environment first
3. **Review requirements** - Some settings may break functionality you need
4. **Check Group Policy** - Domain-joined servers may have GPO-controlled settings

### Potential Impact
- **RDP Settings**: NLA enforcement may block older clients
- **SMB Changes**: May affect legacy systems/applications
- **TLS Changes**: May break connections to older systems
- **Service Changes**: May affect dependent applications

### Domain-Joined Servers
Many settings may be controlled by Group Policy. The script will:
- Warn you about domain membership
- Some settings may not persist after GPO refresh
- Consider implementing via GPO instead for consistency

## 📚 Standards Referenced

| Standard | Description |
|----------|-------------|
| CIS Benchmark | Center for Internet Security Windows Server 2022 Benchmark v1.0+ |
| Microsoft Baselines | Microsoft Security Compliance Toolkit |
| NIST | SP 800-53, SP 800-171 |
| DISA STIG | Defense Information Systems Agency Security Technical Implementation Guides |

## 🔄 Recommended Workflow

1. **Initial Assessment**
   ```powershell
   .\Windows_Server_2022_Hardening.ps1 -Mode CheckOnly -GenerateReport
   ```

2. **Review Report** - Analyze findings and plan remediation

3. **Apply Fixes Interactively**
   ```powershell
   .\Windows_Server_2022_Hardening.ps1 -Mode Interactive
   ```

4. **Verify Changes**
   ```powershell
   .\Windows_Server_2022_Hardening.ps1 -Mode CheckOnly
   ```

5. **Schedule Regular Audits** - Run monthly or after changes

## 🛠️ Customization

### Excluding Specific Checks
Comment out function calls in the `Main` function:
```powershell
# Set-IISConfiguration  # Skip IIS checks
```

### Adding Custom Checks
Add new functions following the existing pattern:
```powershell
function Check-CustomSetting {
    Write-Section "CUSTOM CHECK"
    # Your logic here
    if ($Issue) {
        Write-Issue "Description" -Category "Custom" -Severity "Medium"
    }
}
```

## 📝 Sample Output

```
╔══════════════════════════════════════════════════════════════════════════════╗
║          Windows Server 2022 Security Hardening Script v1.0                  ║
║          Based on: CIS Benchmarks | Microsoft Baselines | NIST              ║
╚══════════════════════════════════════════════════════════════════════════════╝

================================================================================
  2. ACCOUNT POLICIES (CIS 1.1-1.2)
================================================================================

--- Password Policy ---
[OK] Password history: 24 (compliant)
[ISSUE] Maximum password age is 999 days (should be 365 or less)
Set maximum password age to 90 days? (Y/N): y
[FIXED] Set maximum password age to 90 days
[OK] Minimum password age: 1 days (compliant)
[OK] Minimum password length: 14 (compliant)
```

## 🤝 Contributing

Contributions welcome! Areas for improvement:
- Additional CIS benchmark coverage
- Support for older Windows Server versions
- Integration with SCCM/Intune
- Additional reporting formats

## 📄 License

Free to use and modify for security auditing and hardening purposes.

## ⚖️ Disclaimer

This script is provided as-is for educational and security purposes. Always:
- Test in a non-production environment first
- Maintain current backups
- Understand the implications of each change
- Consult with your security team
- Comply with your organization's change management processes

The authors are not responsible for any system issues resulting from the use of this script.
