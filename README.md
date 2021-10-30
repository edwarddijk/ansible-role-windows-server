# Basic Windows Server Configuration

## Requirements

- Microsoft Windows Server 2019
- An Internet Connection
- Python Modules
    - PyWinRM
    - requests-credssp
- Ansible Collections
    - community.windows
    - chocolatey.chocolatey

### Install Ansible Requirements

```bash
ansible-galaxy install -r requirements.yaml
```

## Variables
| Name | Type |Description | Default | 
|------|------|-------|---------|
| cdrom_drive | Boolean | Is a CD-ROM Drive available in the system | yes |
| cdrom_drive_letter | Character | The driveletter to configure for the CD-ROM Drive | Z |
| pagefile_size | Integer | Size of the pagefile in megabyte to be configured | System Memory in MB |
| remote_desktop | Boolean | Enable the Remote Desktop for Admins feature | no |
| staging_directory | String | Storage directory for files needed during install and configuration | C:\Install |
| sysmon_config_url | String | Web location for the Sysmon config xml | https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml |
| print_server | Boolean | Is the server a Print Server | no |
| region_location | integer | Decimal value og geographical location | 176 |
| region_language | string | Locale name | nl_NL |
| region_unicode_language | string | Locale name for unicode | nl_NL |
| timezone | string | timezone name | W. Europe Standard Time |


## Microsoft Language codes etc
https://docs.microsoft.com/en-us/cpp/c-runtime-library/language-strings?view=msvc-160

## Structure
Information : https://stackoverflow.com/questions/55773505/where-to-place-requirements-yml-for-ansible-and-use-it-to-resolve-dependencies

## Hardening (WIP)
Integrating CIS with the help of:
https://github.com/0x6d69636b/windows_hardening

There are some exclusions needed to keep WinRM and a Remote Session working.

### Path: Computer Configuration/Administrative Templates/Windows Components/Windows Remote Management (WinRM)/WinRM Service
- Allow remote server management through WinRM -> Enabled

### Path: Computer Configuration/Administrative Templates/Windows Components/Windows 
- Remote Shell -> Enabled

### Path: Computer Configuration/Security Settings/Local Policies/Security Options
- Network access: Do not allow storage of passwords and credentials for network authentication -> Disabled

Needed for elevated permissions and for example the installation of Microsoft Exchange and or Microsoft SQL Server

### roles\requirements.yaml

```yaml
---    
- src: https://github.com/edwarddijk/ansible-windows-server.git
  scm: git
  version: master
  name: windowsserver
```

```bash
ansible-galaxy install -r roles/requirements.yaml -p ./roles/
```

## Configuration Items

- Install Windows Updates
- Download Powershell DSC Modules
- Set Pagefile
- Set Hostname
- Set Power Settings
- Set Region and Language options
- Set General Network Settings
  - Disable LMHOST Lookup
  - Disable NetBIOS over TCP/IP
- Installing Basic Software with Chocolatey
- Configure Microsoft Sysmon with a (xml)Profile
- Disable Server Manager Scheduled Task
- Change the CD-ROM Drive Letter
- (WIP) Storage Sense Settings?
