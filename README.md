# Linux Security Hardening Script

A comprehensive security audit and hardening script for Ubuntu and Linux Mint, based on the CyberPatriot checklist.

## Features

This script automates the following security checks and fixes:

| Section | Description |
|---------|-------------|
| 1. System Logs | Analyzes /var/log and user bash history for suspicious activity |
| 2. Software Audit | Detects potentially dangerous tools (netcat, nmap, john, etc.) |
| 3. Cron Jobs | Reviews scheduled tasks for unauthorized entries |
| 4. User Policy | Configures password policies, disables guest/auto-login |
| 5. Firewall | Installs and enables UFW with secure defaults |
| 6. Anti-Malware | Installs and runs chkrootkit, rkhunter, ClamAV |
| 7. Auditing | Configures auditd, checks /etc/hosts file |
| 8. Services | Identifies and disables dangerous services |
| 9. Ports | Scans for suspicious open ports |
| 10. Server Config | Hardens Apache2 and SSH configurations |
| 11. Permissions | Fixes critical file and directory permissions |
| 12. Additional | USB automount, media files, kernel parameters |

## Installation

```bash
# Download the script
# Make it executable
chmod +x linux_security_hardener.sh
```

## Usage

**Important:** This script must be run as root (with sudo).

### Interactive Mode (Recommended for first run)
```bash
sudo ./linux_security_hardener.sh
```
You will be prompted before each change is made.

### Check-Only Mode (Safe audit)
```bash
sudo ./linux_security_hardener.sh --check-only
```
Performs all checks but makes no changes. Great for initial assessment.

### Auto-Fix Mode (For experienced users)
```bash
sudo ./linux_security_hardener.sh --auto-fix
```
Automatically fixes all detected issues without prompting.

## What the Script Does

### Password Policy Configuration
- Sets PASS_MAX_DAYS to 90
- Sets PASS_MIN_DAYS to 0  
- Sets PASS_WARN_AGE to 7
- Adds password history (remember=5)
- Sets minimum length (minlen=8)
- Configures account lockout (5 attempts, 30 min lockout)

### User Account Security
- Disables automatic login
- Disables guest account
- Checks for unauthorized users with UID 0
- Reviews sudo/admin group membership

### Services Disabled
- telnet, rsh, rlogin, rexec
- tftp, finger, nis
- avahi-daemon (if not needed)

### Ports Flagged as Suspicious
- 20-21 (FTP)
- 23 (Telnet)
- 135 (RPC)
- 137-139, 445 (SMB)
- 411-412 (P2P)
- 3389 (RDP)
- 5900 (VNC)

## Output

The script generates:
1. **Color-coded console output**
   - 🟢 Green: OK/Fixed
   - 🔴 Red: Issues found
   - 🟡 Yellow: Warnings
   - 🔵 Blue: Information

2. **Log file**: `/var/log/security_hardening_YYYYMMDD_HHMMSS.log`

3. **Backup directory**: `/root/security_backups_YYYYMMDD_HHMMSS/`
   - Contains copies of all modified files

## Example Output

```
╔═══════════════════════════════════════════════════════════════════╗
║         Linux Security Hardening Script v1.0                      ║
║         For Ubuntu / Linux Mint                                   ║
║         Based on CyberPatriot Checklist                           ║
╚═══════════════════════════════════════════════════════════════════╝

═══════════════════════════════════════════════════════════════════
  5. FIREWALL CONFIGURATION
═══════════════════════════════════════════════════════════════════

--- Checking UFW status ---
[✓] UFW is installed
[✗] UFW is not enabled
Enable UFW firewall? (y/n): y
[FIXED] Enabled UFW with default deny incoming policy
```

## Post-Script Recommendations

After running the script, you should manually:

1. **Review user accounts** - Remove any unauthorized users
2. **Update packages** - `sudo apt update && sudo apt upgrade`
3. **Review cron jobs** - Check `/var/spool/cron/crontabs/`
4. **Review installed software** - Use `dpkg -l` or Synaptic
5. **Set strong passwords** - For all user accounts
6. **Review firewall rules** - Customize for your services

## Safety Features

- Creates backups of all files before modification
- Interactive mode by default (requires confirmation)
- Check-only mode available for safe auditing
- Detailed logging of all actions

## Requirements

- Ubuntu 18.04+ or Linux Mint 19+
- Root/sudo access
- Internet connection (for installing packages)

## Troubleshooting

### Script won't run
```bash
chmod +x linux_security_hardener.sh
```

### Permission denied
```bash
sudo ./linux_security_hardener.sh
```

### Package installation fails
```bash
sudo apt update
sudo ./linux_security_hardener.sh
```

## Disclaimer

This script is designed for educational purposes (CyberPatriot competitions) and authorized security auditing. Always:
- Backup your system before running
- Test in a non-production environment first
- Review changes before applying in auto-fix mode
- Understand each change being made

## License

Free to use and modify for educational and security purposes.

