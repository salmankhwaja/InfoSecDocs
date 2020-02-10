# Install Certificate in Windows OS
This post is continuation of move from HTTP to HTTPS series. This post won’t go into the details of how to generate CSR, and submitting the request to Certificate Authority. The process is straight forward.


- [Install Certificate in Windows OS](#install-certificate-in-windows-os)
    + [Convert Certificate files to PFX File](#convert-certificate-files-to-pfx-file)
    + [Install PFX Certificate in Web Server](#install-pfx-certificate-in-web-server)
    + [Install Intermediate Certificates by Certificate Authorities (Optional)](#install-intermediate-certificates-by-certificate-authorities--optional-)
    + [Bind installed Certificates to Default Website / IIS](#bind-installed-certificates-to-default-website---iis)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>



One has to generate the CSR (Certificate Signing Request) for the Server. The key thing to keep in mind is to match the Server Name. (See this post on generating CSR)
Then that CSR is submitted to Certificate Authority to provide Certificate.
Certificate Authority generates the Certificate (either through domain validation or Organization Validation, depending on cost) and provide it over Email / Upload to FTP.
Those Certificates along with Intermediate Certificates are then installed on Web Server.
Once installed, we need to bind the Certificate with the Web Site.
This post discusses the steps 4 – 5 mentioned above. Following are the 4 steps one needs to take if they have received the Certificate from Certificate Authority.

Lastly, CONFIRM if everything is all SET.

Ready. Set. Let’s dive in.


### Convert Certificate files to PFX File
Ok, so we assume that you have Certificate along with it’s Key file. Use the following OpenSSL command to generate the PFX file, which will then be imported into IIS (Internet Information Services).

1

```
openssl pkcs12 -inkey key_name.key -in Certificate_name.crt -export -out certificate_name.pfx
```
Once you have the Certificate it’s time to install it over Web Server.

### Install PFX Certificate in Web Server
Move the Certificates, Key and PFX files over to the Server. Follow these steps.

1. Right click the PFX file and choose INSTALL Certificate as shown in the pic below
capture
2. Another window will pop up. Choose “Local Machine”
3. Navigate the Certificate (PFX file generated above)
4. It might ask for PassPhrase if you have documented the passphrase at the time of requesting the CSR. You will need to enter the pass phrase. Choose the option “Mark this key as exportable”.
5. Choose the option “Automatically select the certificate store based on the type Certificate“
6. Confirm the information by pressing NEXT

If everything is done right, the message Certificate Import is successful should be displayed,

### Install Intermediate Certificates by Certificate Authorities (Optional)
1. on your Keyboard, type Windows + R followed by “MMC“
2. Goto File > Add remove Snap In
3. Choose CERTIFICATES > Add
4. A wizard will open. Choose “Computer Account”,  followed by clicking NEXT, Choose “Local Computer” and finally FINISH
5. Confirm by Click OK
6. Now expand the Certificates Node followed by Intermediate Certificates > Certificates
7. Right click on Certificate node, and choose All Tasks > Import

capture13

8. A Certificate Import Wizard will open
9. Navigate to Intermediate Certificate. Click NEXT
10. Choose default options. Click NEXT
11. Confirm the details. Click NEXT

If everything is done right, the message Certificate Import is successful should be displayed
Repeat STEP 7 – 13 for another Certificate.

### Bind installed Certificates to Default Website / IIS
Now head over the Internet Information Services to deploy the Certificate. Follow the below steps.

1. Press START followed by Run and type Inetmgr in run dialog. Press Enter. Alternatively one can goto start and then type “Internet Information Services“.
2. Right click the machine name, followed by clicking FILTER in contents area.
3. Type “Server Certificates“
4. Double click Server Certificates.
5. Confirm that the Certificate imported above would be displayed here.
6. If confirmed, then click Default Web Site in left side area, followed by Bindings.
7. Choose HTTPS from the drop down and then choose Certificate from the list of Certificate as shown the picture below. Ensure, that HOSTNAME is matched with Server Name, and it is in the form of ServerName.Domainname.com and require Server Name Indication checkbox is checked as illustrated below

capture12

8. Click Ok.

Confirm if the Certificate is properly deployed by opening the Browser and accessing Https://[MachineName]/

Below screen should be displayed.

capture20-1

 

References:
Microsoft, How to configure intermediate certificates on a computer that is running IIS for server authentication,  https://support.microsoft.com/en-us/help/954755/how-to-configure-intermediate-certificates-on-a-computer-that-is-running-iis-for-server-authentication

Go Daddy, IIS 7: Install a certificate, https://pk.godaddy.com/help/iis-7-install-a-certificate-4801
