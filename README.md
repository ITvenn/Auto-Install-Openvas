# Greenbone OpenVAS Scanner Build Script

### 📦 Auto setup link
**(Do NOT run as root, scripts will prompt for sudo)**
```bash
wget https://raw.githubusercontent.com/itiligent/Easy-OpenVAS-Installer/main/openvas-install.sh && chmod +x openvas-install.sh && ./openvas-install.sh
```

### 📦 Auto upgrade link
```bash
wget https://raw.githubusercontent.com/itiligent/Easy-OpenVAS-Installer/main/openvas-upgrade.sh && chmod +x openvas-upgrade.sh  && ./openvas-upgrade.sh 
```

### 📋 Prerequisites

**Script defaults build OpenVAS from latest release source**
- **Supported OS:**
  - Ubuntu 22.04 (Preferred by Greenbone) | 23.04 | 24.04 
  - Debian 11.x | 12.x 
  - Raspbian (Buster | Bookworm)
- **Required Packages**:
  - curl & sudo 
- **Hardware Requirements**:
  - Minimum 8GB RAM
  - Minimum 80GB Storage
- **Network Requirements**:
  - IPv6 must be enabled
  - Avoid multiple NICs
- **Permissions**:
  - Run script as a user account with sudo rights, do not run as root. 🛡️
- **Optional**:
  - Email PDF scan reports require an O365 email-enabled account & email relay permitted from the scanner IP address
  - A private DNS entry for HTTPs console access

### 📖 Customising Your Build
All script options are managed in the `CUSTOM CONFIG SETTINGS` at the top of the scripts.  Both the install and upgrade scripts will check GitHub for the latest release of each module. Specific module versions can be forced in the `OVERRIDE LATEST RELEASE AUTO SELECTION` section.

### 📧 Adding Email Reporting To Community Edition
*(normally a pro version feature)*

A Postfix MTA is installed by default. Configure the included template script `add-email-reports-o365.sh` with your own O365 app password and mail relay configuration.

### ⬆️ Upgrading & Updating OpenVAS

- A CVE feed update is scheduled by the installer once daily at a random time, and this can be adjusted via cron.
- To upgrade the OpenVAS, run  `openvas-upgrade.sh` from the link above.

### 🔒 HTTPS Web Console Access 

The install script automatically configures an HTTP redirect to port 443 and configures the required TLS certificates based on options in the `CUSTOM CONFIG SETTINGS` section. Instructions for importing browser certificates into Windows and Linux clients (to avoid browser HTTPS warnings) are provided on-screen when the install script completes. If you wish you change the system's DNS name or IP address, or to renew certificates, run [update-certificates.sh](https://github.com/itiligent/Easy-OpenVAS-Installer/blob/main/update-certificates.sh)


### 💻 How To Run Authenticated Scans Against Windows Hosts

To perform vulnerability scans against Windows hosts using SMB authentication, follow these steps:

1. Run the included PowerShell script [prepare-smb-cred-scan.ps1](https://github.com/itiligent/OpenVAS-Appliance-Builder/blob/main/prepare-smb-cred-scan.ps1) to configure Windows systems for scanning with SMB credentials. *(The pro version provides an equivalent .exe for mass deployment)*
2. Create a GVM service account and add this account to the local Administrators group on each host (this service account must NOT be a built-in Windows account).
3. Create a new credentials configuration object in the management console reflecting the new Windows service account's details.
4. Add Windows hosts to a new scanning target, then select the new credetials object created above under "Credentials for authenticated checks" to apply these credentials to the scan.
5. Create a new scanning task for the scan target(s) above, then run or schedule the scan.
