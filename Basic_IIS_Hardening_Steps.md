
# Instructions document for Hardening
## Disable Directory Browsing in IIS

1. Open IIS manager and navigate to the level you to manage
2. In features view ,double click **Directory browsing**
3. In the Actions pane, click **Disable** if the Directory Browsing feature is enabled.

## Disable OPTIONS and TRACE METHOD in IIS and Enable Get, Post, Head.

1. Open IIS manager and navigate to the level you to manage
2. In features view ,double click **Request Filtering**
3. Click the *HTTP VERBS*. Remove all the VERBS one by one. 
4. Under ALLOW VERB, add "GET", "POST", "HEAD"
5. Under DENY VERB, add "OPTIONS", "TRACE", "PATCH", "PUT", "DELETE"
6. Run Command line from Administrator and then issue "IISRESET" to restart the IIS
7. 

## Enable X-Frame-Options in IIS.

1. Open **Internet Information Services (IIS)** Manager. START > RUN > inetmgr
2. In the Connections pane on the left side, expand the Sites folder and select the site that you want to protect.
3. Double-click the **HTTP Response Headers** icon in the feature list in the middle.
4. In the Actions pane on the right side, click **Add**.
5. In the dialog box that appears, type **X-Frame-Options** in the Name field and type **SAMEORIGIN** or **DENY** in the Value field.
6. Click OK to save your changes.
7. Run Command line from Administrator and then issue "IISRESET" to restart the IIS

## Enable XSS-Protection in IIS.
1. Open **Internet Information Services (IIS)** Manager. START > RUN > inetmgr
2. In the Connections pane on the left side, expand the Sites folder and select the site that you want to protect.
3. Double-click the **HTTP Response Headers** icon in the feature list in the middle.
4. In the Actions pane on the right side, click **Add**.
5. In the dialog box that appears, type **X-XSS-Protection** in the Name field and type **1; mode=block** in the Value field.
6. Click **OK** to save your changes.
7. Run **Command line** from Administrator and then issue **"IISRESET"** to restart the IIS


## Enable Content-Security-Policy in IIS.
1. Open **Internet Information Services (IIS)** Manager. START > RUN > inetmgr
2. In the Connections pane on the left side, expand the Sites folder and select the site that you want to protect.
3. Double-click the **HTTP Response Headers** icon in the feature list in the middle.
4. In the Actions pane on the right side, click **Add**.
5. In the dialog box that appears, type **Content-Security-Policy** in the Name field and type **default-src 'none'; script-src 'self'; connect-src 'self'; img-src 'self'; style-src 'self';** in the Value field.
**NOTE:** The Content Security Policy is the Starter Policy for any application. It is generally recommended to add a custom Content Security Policy as per needs. 
6. Click **OK** to save your changes.
7. Run **Command line** from Administrator and then issue **"IISRESET"** to restart the IIS

Links to Read
https://content-security-policy.com/
https://rehansaeed.com/content-security-policy-for-asp-net-mvc/
https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP


## Enable Stirct-Transport-Security in IIS.
1. Open **Internet Information Services (IIS)** Manager. START > RUN > inetmgr
2. In the Connections pane on the left side, expand the Sites folder and select the site that you want to protect.
3. Double-click the **HTTP Response Headers** icon in the feature list in the middle.
4. In the Actions pane on the right side, click **Add**.
5. In the dialog box that appears, type **Strict-Transport-Security** in the Name field and type **max-age=31536000; includeSubDomains** in the Value field.
6. Click **OK** to save your changes.
7. Run **Command line** from Administrator and then issue **"IISRESET"** to restart the IIS


## Enable X-Content-Type-Options in IIS.
1. Open **Internet Information Services (IIS)** Manager. START > RUN > inetmgr
2. In the Connections pane on the left side, expand the Sites folder and select the site that you want to protect.
3. Double-click the **HTTP Response Headers** icon in the feature list in the middle.
4. In the Actions pane on the right side, click **Add**.
5. In the dialog box that appears, type **X-Content-Type-Options** in the Name field and type **nosniff** in the Value field.
6. Click **OK** to save your changes.
7. Run **Command line** from Administrator and then issue **"IISRESET"** to restart the IIS

## Suppress Web Server in IIS.

1. Open **Internet Information Services (IIS)** Manager. START > RUN > inetmgr
2. In the Connections pane on the left side, expand the *Sites* folder and select the site that you want to protect.
3. Double-click the **HTTP Response Headers** icon in the feature list in the middle.
4. Confirm if there is a Header by the name of **SERVER**.
6. Remove it by clicking **REMOVE** from the actions pane
7. Run Command line from Administrator and then issue "IISRESET" to restart the IIS

## Disable Default IIS document in IIS.

1. In Run, type **“inetmgr”**. This will open IIS.
2. Go to **Sites | Default Web Site** and select **Default Document** property.
3. Double click on **Default Document** property. This will open Default document pages.
4. Right click on **Default.htm** page and click on **disable**.

From https://www.greytrix.com/blogs/sagecrm/2013/03/29/how-to-restrict-users-from-accessing-default-iis-page-and-virtual-directory/
and 
https://docs.microsoft.com/en-us/iis/web-hosting/web-server-for-shared-hosting/default-documents


## Check your headers. 
lastly check your headers from https://securityheaders.io/. 

Read more about it here. 

https://www.troyhunt.com/shhh-dont-let-your-response-headers/
https://blogs.msdn.microsoft.com/varunm/2013/04/23/remove-unwanted-http-response-headers/

Also, search for the httpRuntime string in Web.Config and set the attribute enableVersionHeader it to FALSE.
```
<system.web>
  <httpRuntime enableVersionHeader="false" />
</system.web>
```
One could also follow along this link to remove un wanted Server header. 
https://blogs.msdn.microsoft.com/varunm/2013/04/23/remove-unwanted-http-response-headers/


# Security Headers

Security Headers provide a very powerful way to further harden your Web application. The following Security Headers should be added in your applications Web.Config file. 


```
<! — Start Security Headers →
 <httpProtocol>
 <customHeaders>
 <add name=”X-XSS-Protection” value=”1; mode=block”/>
 <add name=”Content-Security-Policy” value=”default-src ‘self’”/>
 <add name=”X-frame-options” value=”SAMEORIGIN”/>
 <add name=”X-Content-Type-Options” value=”nosniff”/>
 <add name=”Referrer-Policy” value=”strict-origin-when-cross-origin”/>
 <remove name=”X-Powered-By”/>
 </customHeaders>
 </httpProtocol>
 <! — End Security Headers →
 ```
 
 
# Host Header Remediation
Host Header detailed description about what is it and how it is exploited can be seen at the URL. 
https://www.acunetix.com/blog/articles/automated-detection-of-host-header-attacks/

For remediation on IIS, follow the below steps.

1. In IIS, click the **Website** you need to change. 
2. In the **ACTIONS** Tab, click **BINDINGS**
3. Click ADD or Choose the list item and click EDIT.
4. Make sure, you specify the **HostName** to Website or Domain name being called. 
5. Choose **SSL Certificate** as per need. Normally you would choose the Certificate binded with your Website. 
6. Click **OK** to close this dialog box
7. Repeat Steps 3 to 6 if you want another HOST Header. Normally, you would choose this if you have a one Website, and you would like to call it by different URLs.
8. Click **Close** to finish off the settings and 
9. Got to Command line by **"Windows + R"** followed by type **"CMD"**. 
10. Type **"IISRESET"** to restart the IIS with the new settings. 

**PS:** Command line for the same is 
appcmd set site /site.name:"digicert.com" /+bindings.[protocol='https',bindingInformation='*:443:digicert.com']

 
 Image URL
 https://cdn-images-1.medium.com/max/800/1*3AgIok349W9tvYMrnSdPww.png
 
 
 
 This is how Security headers are added in Code.
 
 app.UseHsts(hsts => hsts.MaxAge(365).IncludeSubdomains());
 app.UseXContentTypeOptions();
 app.UseReferrerPolicy(opts => opts.NoReferrer());
 app.UseXXssProtection(options => options.EnabledWithBlockMode());
 app.UseXfo(options => options.Deny());

app.UseCsp(opts => opts
 .BlockAllMixedContent()
 .StyleSources(s => s.Self())
 .StyleSources(s => s.UnsafeInline())
 .FontSources(s => s.Self())
 .FormActions(s => s.Self())
 .FrameAncestors(s => s.Self())
 .ImageSources(s => s.Self())
 .ScriptSources(s => s.Self())
 );
 //End Security Headers


 
 
 # References:
 https://medium.com/@shehackspurple/security-headers-1c770105940b
 https://damienbod.com/2018/02/08/adding-http-headers-to-improve-security-in-an-asp-net-mvc-core-application/
 
 
 
