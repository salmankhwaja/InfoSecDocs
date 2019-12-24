# How to switch off SMB Signing 

This guide is made for switching off SMB Signing in different Operating Systems. Please do keep in mind, that general windows Adminsitration is required to do these tasks. 

nsure that the windows is updated with MS17-010. If this update is not installed, Microsoft provides a temporary workaround to disable the SMB Protocol. Disabling this protocol will impact the functionality of file sharing. The procedure for disabling SMB protocol is listed below.

### For Windows 8 and Windows Server 2012
```
Win + R (in run dialog copy paste the following command to open Powershell as Administrator)
powershell -Command "Start-Process PowerShell –Verb RunAs" 
# To obtain the current state of the SMB server protocol configuration, run the following cmdlet:
Get-SmbServerConfiguration | Select EnableSMB1Protocol, EnableSMB2Protocol
# To disable SMBv1 on the SMB server, run the following cmdlet:
Set-SmbServerConfiguration -EnableSMB1Protocol $false
# To disable SMBv2 and SMBv3 on the SMB server, run the following cmdlet:
Set-SmbServerConfiguration -EnableSMB2Protocol $false
```
### For Windows 7, Windows Server 2008 R2, Windows Vista, and Windows Server 2008

```
Win + R (in run dialog copy paste the following command to open Powershell as Administrator)

powershell -Command "Start-Process PowerShell –Verb RunAs" 

# To disable SMBv1 on the SMB server, run the following cmdlet:
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 0 -Force
# To disable SMBv2 and SMBv3 on the SMB server, run the following cmdlet:
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 0 -Force
# To enable SMBv1 on the SMB server, run the following cmdlet:
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB1 -Type DWORD -Value 1 -Force
# To enable SMBv2 and SMBv3 on the SMB server, run the following cmdlet:
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" SMB2 -Type DWORD -Value 1 -Force
```

### For Windows Vista, Windows Server 2008, Windows 7, Windows Server 2008 R2, Windows 8, and Windows Server 2012

Win + R (in run dialog copy paste the following command to open Powershell as Administrator)

powershell -Command "Start-Process PowerShell –Verb RunAs" 
#To disable SMBv1 on the SMB client, run the following commands:
sc.exe config lanmanworkstation depend= bowser/mrxsmb20/nsi
sc.exe config mrxsmb10 start= disabled
#To enable SMBv1 on the SMB client, run the following commands:
sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi 
sc.exe config mrxsmb10 start= auto
#To disable SMBv2 and SMBv3 on the SMB client, run the following commands:
sc.exe config lanmanworkstation depend= bowser/mrxsmb10/nsi 
sc.exe config mrxsmb20 start= disabled
#To enable SMBv2 and SMBv3 on the SMB client, run the following commands:
sc.exe config lanmanworkstation depend= bowser/mrxsmb10/mrxsmb20/nsi 
sc.exe config mrxsmb20 start= auto
```

### To do the same via the User Interface
On Windows Servers (2008 R2 SP1 / SP2, 2012, 2016)

1. Open **Server Manager**
2. Click **Manage**
3. Click **Remove Roles and Features**
4. In the features window, ensure that **SMB 1.0/ CIFS file sharing support** is **NOT** checked.
5. Click **OK** to close the window
6. **Restart** the system to disable the SMB Protocol

<img src = "https://tpsappsecaware.files.wordpress.com/2017/05/4014204_en_1.png?w=525">


On Windows Clients  (Windows 7, 8, 8.1, 10)

1. Open **Control Panel**
2. Click **Programs and Features** followed by **Turn Windows Features on or off**
3. Ensure that **SMB 1.0/ CIFS file sharing support** is **NOT** checked.
4. Click **OK** to close the window
5. **Restart** the system to disable the SMB Protocol.

<img src = "https://tpsappsecaware.files.wordpress.com/2017/05/capture.png?w=525">

Note, Microsoft provides this fix as a TEMPORARY fix to combat against WannaCry. 
Keep your Windows Updated with Microsoft patches / service packs. They will soon be followed by permanent fix.

From:
https://tpsappsecaware.wordpress.com/2017/05/15/wannacry-ransomware-what-is-it-and-how-to-defend-it/
