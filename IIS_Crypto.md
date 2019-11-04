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
