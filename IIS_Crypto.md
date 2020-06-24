# IISCrypto

IISCrypto is the tool which is used to enable disable the Network protocols of any dektop / server machine. Here is snippet from their site.

> IIS Crypto is a free tool that gives administrators the ability to enable or disable protocols, ciphers, hashes and key exchange algorithms on Windows Server 2008, 2012 and 2016. It also lets you reorder SSL/TLS cipher suites offered by IIS, implement best practices with a single click, create custom templates and test your website.


It is also the effortless way to secure SSL/TLS in Windows.

One could find more details here.

https://www.nartac.com/Products/IISCrypto

This tool updates the registry entries as per documented by Microsoft. 
http://support.microsoft.com/default.aspx?scid=kb;EN-US;245030

Download the IIS Crypto on the machine where Network protocols are to be disbled.

As per PCI,

- all early SSL version must be switched off.
- all early TLS versions, that is TLS 1.0, 1.1 needs to be switched off
- only TLS 1.2 needs to be switched on.

IIS Crypto does all that with one click. Be it GUI mode or command line mode.

Download this tool by clicking the below URLs.

https://www.nartac.com/Products/IISCrypto/Download 

https://www.nartac.com/Downloads/IISCrypto/IISCrypto.exe 

https://www.nartac.com/Downloads/IISCrypto/IISCryptoCli.exe

## Following are the steps to secure SSL / TLS in windows.

1. Download the **IISCrypto** from Links mentioned above.
2. Transfer them to Desktop prefreably.
3. Right click, and choose run as **Administrator**.
4. Once IIS Crypto GUI appears
5. Click **BEST PRACTICES**, followed by **ensuring**, 
    1. **TLS 1.0** and **TLS 1.1** is **checked OFF**. If they are Checked on, then **do Check them off**.
    2. **SSL** are versions are checked off.
    3. Only **TLS 1.2** is switched on
6. Make sure to take backup of Registry by clicking the **REGISTRY** button and taking backup of it.
7. Check **REBOOT**
8. Click **APPLY**.
9. On clicking apply, the IIS Crypto will perform the changes and **ask for reboot**.
10. Allow the system to reboot.

On Reboot, all 

1. SSL would be swtiched off.
2. TLS 1.0 and 1.1 would be switched off
3. TLS 1.2 would be switched on 
4. All cipher suites would be re-ordered as per forward secracy

## How to check if SSL, TLS 1.0, TLS 1.1, TLS 1.2 are enabled or disabled ?

To disable the PCT 1.0 protocol so that IIS does not try to negotiate using the PCT 1.0 protocol, follow these steps:

1. Click Start, click Run, type regedt32 or type regedit, and then click OK.
In Registry Editor, locate the following registry key:
2. HKey_Local_Machine\System\CurrentControlSet\Control\SecurityProviders \SCHANNEL\Protocols\PCT 1.0\Server
3. On the Edit menu, click Add Value.
4. In the Data Type list, click DWORD.
5. In the Value Name box, type Enabled, and then click OK.

Note If this value is present, double-click the value to edit its current value.
Type 00000000 in Binary Editor to set the value of the new key equal to "0".

6. Click OK. Restart the computer.


1. On the Windows server, open the Registry Editor (regedit.exe) and run it as administrator.
2. In the Registry Editor window, go to: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel\Protocols\
3. Note: If the key SSL 3.0 is already existing, skip steps 3 and 4.
In the navigation tree, right-click on Protocols, and in the pop-up menu, click New > Key.
4. Name the key, SSL 3.0.
5. In the navigation tree, right-click on the new SSL 3.0 key that you just created, and in the pop-up menu, click New > Key. Name the key Client.
5. Right-click on Client, and in the pop-up menu, click New > DWORD (32-bit) Value.
6. Name the value DisabledByDefault. Double-click the DisabledByDefault DWORD value and in the Edit DWORD (32-bit) Value window, in the Value Data box change the value to 1 and then click OK.
7. In the navigation tree, right-click on the SSL 3.0 key again, and in the pop-up menu, click New > Key. Name the key Server.
8. Right-click on Server, and in the pop-up menu, click New > DWORD (32-bit) Value. Name the value Enabled. 
9. Double-click the Enabled DWORD value and in the Edit DWORD (32-bit) Value window, in the Value Data box leave the value at 0 and then, click OK.
10. Restart your Windows server.


Below are the key combinations for disabling the SSL 2.0, SSL 3.0 and TLS 1.0 protocols on Windows 10 or Windows 2012 server.

## For SSL 2.0

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel\Protocols\SSL 2.0\Client]
"DisabledByDefault"=dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel\Protocols\SSL 2.0\Server]
"Enabled"=dword:00000000
 

### For SSL 3.0

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel\Protocols\SSL 3.0\Client]
"DisabledByDefault"=dword:00000001
 
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel\Protocols\SSL 3.0\Server]
"Enabled"=dword:00000000

### For TLS 1.0
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel\Protocols\TLS 1.0\Client]
"DisabledByDefault"=dword:00000001
 
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel\Protocols\TLS 1.0\Server]
"Enabled"=dword:00000000

Note: Client portion contains subkey called "DisabledByDefault" whereas the Server portion contains subkey called "Enabled"


References: 
https://support.microsoft.com/en-us/help/187498/how-to-disable-pct-1-0-ssl-2-0-ssl-3-0-or-tls-1-0-in-internet-informat

https://www.venafi.com/education-center/ssl/how-to-check-ssl-certificate#:~:text=Click%20the%20padlock%20icon%20in,the%20SSL%20certificate%20is%20current
