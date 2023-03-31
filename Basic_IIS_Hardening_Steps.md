- [Instructions document for Hardening](#instructions-document-for-hardening)
  * [Disable Directory Browsing in IIS](#disable-directory-browsing-in-iis)
  * [Disable OPTIONS and TRACE METHOD in IIS and Enable Get / Post / Head.](#disable-options-and-trace-method-in-iis-and-enable-get---post---head)
  * [Enable X-Frame-Options in IIS.](#enable-x-frame-options-in-iis)
  * [Enable XSS-Protection in IIS.](#enable-xss-protection-in-iis)
  * [Configure Custom Error pages in IIS.](#configure-custom-error-pages-in-iis)
  * [Implement Secure Referrer Policy](#implement-secure-referrer-policy)
  * [Enable Content-Security-Policy in IIS.](#enable-content-security-policy-in-iis)
  * [Enable Stirct-Transport-Security in IIS. HSTS](#enable-stirct-transport-security-in-iis-hsts)
  * [Enable X-Content-Type-Options in IIS.](#enable-x-content-type-options-in-iis)
  * [Suppress Web Server in IIS.](#suppress-web-server-in-iis)
  * [Remove ASP.NET Version, Server Verion and Other un wanted headers.](#remove-aspnet-version--server-verion-and-other-un-wanted-headers)
  * [Remove ASP.NET 4.5](#remove-aspnet-45)
  * [Disable ASP.NET debug feature (http-asp-dot-net-debug)](#disable-aspnet-debug-feature--http-asp-dot-net-debug-)
  * [Modify the Web.config File](#modify-the-webconfig-file)
  * [Modify the Machine.config File](#modify-the-machineconfig-file)
  * [Removing X-Powered-By Header.](#removing-x-powered-by-header)
  * [Disable Default IIS document in IIS.](#disable-default-iis-document-in-iis)
  * [Cookies (Secure only / Http only)](#cookies--secure-only---http-only-)
  * [Secure Cookies with root path](#secure-cookies-with-root-path)
  * [Check your headers.](#check-your-headers)
  * [Security Headers](#security-headers)
  * [Disable ViewState in Asp.NET Pages](#disable-viewstate-in-aspnet-pages)
  * [Host Header Remediation](#host-header-remediation)
  * [Content Security Policy](#content-security-policy)
  * [References:](#references-)
  * [CORS - Cross Origin Resource Sharing.](#cors---cross-origin-resource-sharing)
  * [Microsoft IIS Tilde Vulnerability](#microsoft-iis-tilde-vulnerability)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


# Instructions document for Hardening
## Disable Directory Browsing in IIS

1. Open IIS manager and navigate to the level you to manage
2. In features view ,double click **Directory browsing**
3. In the Actions pane, click **Disable** if the Directory Browsing feature is enabled.

## Disable OPTIONS and TRACE METHOD in IIS and Enable Get / Post / Head.

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

## Configure Custom Error pages in IIS.
Follow the Steps listed in this link. 
https://docs.microsoft.com/en-us/iis/configuration/system.webserver/httperrors/

1. In the Connections pane, expand the server name, expand **Sites**, and then navigate to the **Web site or application** that you want to configure custom error pages for.
2. In the Home pane, double-click **Error Pages**.
3. In the Actions pane, click **Add...**
4. In the Add Custom Error Page dialog box, under **Status code**, type the **number of the HTTP status code** for which you want to create a custom error message.
5. In the **Response Action section**, do **one** of the following:
   a. Select **Insert content from static file** into the error response to serve static content, for example, an .html file, for the custom error.
   b. Select **Execute a URL** on this site to serve dynamic content, for example, an .asp file for the custom error.
   c. Select **Respond with a 302 redirect** to redirect client browsers to a different URL that contains the custom error file.
6. In the File path text box, **type the path of the custom error page** if you chose Insert content from static file into the error response or the **URL of the custom error page** if you use either the Execute a URL on this site or Respond with a **302 redirect**, and then click **OK**.

One command line for this work to be performed is
```
appcmd.exe set config -section:system.webServer/httpErrors /+"[statusCode='404',subStatusCode='5',prefixLanguageFilePath='%SystemDrive%\inetpub\custerr',path='404.5.htm']" 
/commit:apphost
```


## Implement Secure Referrer Policy
The documentation for Referrer policy is @ https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy. It should be implemented using HTTP Headers. 
1. Open **Internet Information Services (IIS)** Manager. START > RUN > inetmgr
2. In the Connections pane on the left side, expand the Sites folder and select the site that you want to protect.
3. Double-click the **HTTP Response Headers** icon in the feature list in the middle.
4. In the Actions pane on the right side, click **Add**.
5. In the dialog box that appears, type **Referrer-Policy** in the Name field and type **strict-origin-when-cross-origin** in the Value field. One could also choose required values from the documentation provided above.   
6. Click **OK** to save your changes.
7. Run **Command line** from Administrator and then issue **"IISRESET"** to restart the IIS

Alternatively, one could choose to edit the relevant Web.Config file add these header. 

<httpProtocol>
    <customHeaders>
      <!-- SECURITY HEADERS - https://securityheaders.io/? -->
      <!-- Protects against Clickjacking attacks. ref.: http://stackoverflow.com/a/22105445/1233379 -->
      <!-- Protects against MIME-type confusion attack. ref.: https://www.veracode.com/blog/2014/03/guidelines-for-setting-security-headers/ -->
      <add name="Referrer-Policy" value="strict-origin" />
    </customHeaders>
  </httpProtocol>
</system.webServer>


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


## Enable Stirct-Transport-Security in IIS. HSTS
1. Open **Internet Information Services (IIS)** Manager. START > RUN > inetmgr
2. In the Connections pane on the left side, expand the Sites folder and select the site that you want to protect.
3. Double-click the **HTTP Response Headers** icon in the feature list in the middle.
4. In the Actions pane on the right side, click **Add**.
5. In the dialog box that appears, type **Strict-Transport-Security** in the Name field and type **max-age=31536000; includeSubDomains** in the Value field.
6. Click **OK** to save your changes.
7. Run **Command line** from Administrator and then issue **"IISRESET"** to restart the IIS


Also, the following XML could be added in for Re-Direction from HTTP to HTTPS. 

```
<httpProtocol>
    <customHeaders>
      <!-- SECURITY HEADERS - https://securityheaders.io/? -->
      <!-- Protects against Clickjacking attacks. ref.: http://stackoverflow.com/a/22105445/1233379 -->
      <!-- Protects against MIME-type confusion attack. ref.: https://www.veracode.com/blog/2014/03/guidelines-for-setting-security-headers/ -->
      <add name="Referrer-Policy" value="strict-origin" />
    </customHeaders>
  </httpProtocol>
</system.webServer>
```
```
<system.webServer>
        <httpRedirect enabled="true" destination="https://MachineName.DomainName.com/URI" httpResponseStatus="Permanent" />
</system.webServer>
```


HSTS Header could also be added in Web.Config
```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <httpProtocol>
            <customHeaders>
                <add name="Strict-Transport-Security" value="max-age=31536000" />
            </customHeaders>
        </httpProtocol>
    </system.webServer>
</configuration>
```
Read this link for more information. 

https://docs.microsoft.com/en-us/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#:~:text=HTTP%20Strict%20Transport%20Security%20(HSTS)%2C%20specified%20in%20RFC%206797,contacted%20only%20through%20HTTPS%20connections.



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

## Remove ASP.NET Version, Server Verion and Other un wanted headers. 
Detailed instructions provided here. 
https://blog.insiderattack.net/configuring-secure-iis-response-headers-in-asp-net-mvc-b38369030728

## Remove ASP.NET 4.5
1. Open the Web.Config file, 
2. find the node **<httpRuntime>** under **<system.web>** 
3. add the **enableVersionHeader** attribute to **httpRuntime** node and set it to **false**.
 
 <httpRuntime maxRequestLength="4096" targetFramework="4.5" enableVersionHeader="false"/>

## Disable ASP.NET debug feature (http-asp-dot-net-debug)

Detailed instructions are @ https://support.microsoft.com/en-us/help/815157/how-to-disable-debugging-for-asp-net-applications

To disable debugging, modify either the **Web.config file** or the **Machine.config file**, as detailed in the following steps.
## Modify the Web.config File
To enable debugging, add the compilation element to the Web.config file of the application. The Web.config file is located in the application directory. To do this, follow these steps:

1. Open the Web.config file in a text editor such as Notepad.exe. Web.config file is typically located in the application directory.
2. In the Web.config file, locate the **compilation** element. Debugging is enabled when the debug attribute in the compilation element is set to true.
3. Modify the debug attribute to false, and then save the Web.config file to **disable debugging** for that application.

   The following code sample shows the compilation element with debug set to false:
   ```
   <compilation 
       debug="false"
   />
   ```
4. Save the Web.config file. The ASP.NET application automatically restarts.

## Modify the Machine.config File
You can also enable debugging for all applications on a system by modifying the Machine.config file. To confirm that debugging has not been enabled in the Machine.config file, follow these steps.

1. Open the Machine.config file in a text editor such as Notepad.exe. The Machine.config file is typically located in the following folder:
%SystemRoot%\Microsoft.NET\Framework\%VersionNumber%\CONFIG\
2. In the Machine.config file, locate the **compilation** element. Debugging is enabled when the debug attribute in the compilation element is set to true.
If the debug attribute is true, change the debug attribute to false.
3. The following code sample shows the compilation element with debug set to false:

  ```
  <compilation 
    debug="false"
  />
  ```
4. Save the Machine.config file.


## Removing X-Powered-By Header.

1. Open the Web.Config file, 
2. find the <httpProtocol> node under the <system.webServer> node. 
3. Check whether these is a child node under <httpProtocol> called <customHeaders>. By default in MVC, you will not see this customHeaders child node. If it does not exist, create a <cusstomHeaders> node and add following include following to remove X-Powered-By header.
 
```
<httpProtocol> 
 <customHeaders> 
  <remove name="X-Powered-By"/>
 </customHeaders> 
</httpProtocol>
```


## Disable Default IIS document in IIS.

1. In Run, type **“inetmgr”**. This will open IIS.
2. Go to **Sites | Default Web Site** and select **Default Document** property.
3. Double click on **Default Document** property. This will open Default document pages.
4. Right click on **Default.htm** page and click on **disable**.

From https://www.greytrix.com/blogs/sagecrm/2013/03/29/how-to-restrict-users-from-accessing-default-iis-page-and-virtual-directory/
and 
https://docs.microsoft.com/en-us/iis/web-hosting/web-server-for-shared-hosting/default-documents

## Cookies (Secure only / Http only)
The application cookies needs to be set as SECURE and HttpOnly Flag. The following line is to be added in Web.Config to make the Cookies Secure.
The line below is quite relaxed, but will make the cookies Secure and HTTPOnly.
Add this line within Web.config  within <system.web>:

```
<httpCookies httpOnlyCookies="true" requireSSL="true" />
```

## Secure Cookies with root path

Root path is generally added in cookies to make cookies for that particular path / domain. Best approach is to use the path directive and domain directive.

```
<httpCookies httpOnlyCookies="true" requireSSL="true" path="/" domain="xyz.*.com" />
```
Read the following links for further understanding

https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie
https://wiki.owasp.org/index.php/Testing_for_cookies_attributes_(OTG-SESS-002)
https://headmelted.com/securing-asp-net-cookies-a1e1b1648ed


## Check your headers. 
Lastly check your headers from https://securityheaders.io/. 

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


## Security Headers

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
 <add name=”Cache-Control" value="no-store, no-cache, must-revalidate"/>
 <remove name=”X-Powered-By”/>
 </customHeaders>
 </httpProtocol>
 <! — End Security Headers →
 ```

## Disable ViewState in Asp.NET Pages
1. Open desired Web.Config file and go to <system.web> tag.
2. Add the following tag in <system.web>

```
<system.web>
 <page enableViewStateMac="true" viewStateEncryptionMode="Always">
 </pages> 
</system.web>
 ```
 or 
 ```
 <system.web>
 <pages enableViewState="false" />
 </system.web>
 ```
 
 
 
 3. ReStart the Webserver by issuing "cmd" from command line and issues "IISRESET" from Administrator command prompt.
 
 
## Host Header Remediation
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




## Content Security Policy
Content Security Policy is the header which is applied in applications. With this policy in place, application developers could enable / disable many areas of their Web site to be executed in browser environment. Things like Links, Cross Links, Images, JavaScripts, resources lying in their server and / or in partner's Servers. In other words, it allows to define a policy to run on major browsers, which will be a defense mechanism for Websites. 

More details here. https://content-security-policy.com/

For remediation on IIS, follow the below steps.

1. Open **Internet Information Services (IIS)** Manager. START > RUN > inetmgr
2. In the Connections pane on the left side, expand the Sites folder and select the site that you want to protect.
3. Double-click the **HTTP Response Headers** icon in the feature list in the middle.
4. In the Actions pane on the right side, click **Add**.
5. Add the following key value pairs one by one repeating 4 and 5.

    Content-Security-Policy 
    default-src 'self'; connect-src 'self'; font-src 'self'; img-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline'; object-src 'none'

    X-Content-Security-Policy
    default-src 'self'; connect-src 'self'; font-src 'self'; img-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline'; object-src 'none'

    X-WebKit-CSP
    default-src 'self'; connect-src 'self'; font-src 'self'; img-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline'; object-src 'none'


6. Click OK to save your changes.
7. Run Command line from Administrator and then issue "IISRESET" to restart the IIS

**PS:** Command line for the same is 
appcmd set site /site.name:"digicert.com" /+bindings.[protocol='https',bindingInformation='*:443:digicert.com']

 
 
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

 
 ## References:
 https://medium.com/@shehackspurple/security-headers-1c770105940b
 https://damienbod.com/2018/02/08/adding-http-headers-to-improve-security-in-an-asp-net-mvc-core-application/
 
 
 
## CORS - Cross Origin Resource Sharing. 

Cross-origin resource sharing (CORS) is a browser mechanism which enables controlled access to resources located outside of a given domain. It extends and adds flexibility to the same-origin policy (SOP). However, it also provides potential for cross-domain based attacks, if a website's CORS policy is poorly configured and implemented. CORS is not a protection against cross-origin attacks such as cross-site request forgery (CSRF).

One could the following CORS headers in IIS and / or webserver of their choice. The following are the types of CORS Headers, which should be used. 

  Access-Control-Allow-Origin: http://foo.example 

  Access-Control-Allow-Methods: POST, GET, OPTIONS

  Access-Control-Allow-Headers: X-PINGOTHER, Content-Type

  Access-Control-Max-Age: 86400

  Access-Control-Allow-Credentials: true

  Access-Control-Expose-Headers: None

  Access-Control-Request-Method: POST, GET, OPTIONS

  Access-Control-Request-Headers: X-PINGOTHER, Content-Type

The other options are self explanatory, but Access-Control-Max-Age gives the value in seconds for how long the response to the preflight request can be cached for without sending another preflight request. In this case, 86400 seconds is 24 hours. 

Do the following to add your required CORS Headers in IIS. 

1. Open Internet Information Service (IIS) Manager
2. **Right click** the site you want to enable CORS for and go to **Properties**
3. Change to the **HTTP Headers** tab
4. In the Custom HTTP headers section, click **Add**
5. Enter **Access-Control-Allow-Origin** as the header name
6. Enter *https://domain.url.com*  as the header value. In case of Multiple headers, use the method below of adding in URLs displayed via XML highlighed below.
7. Click **Ok** twice

Alternatively, the following snippet of WebConfig can be dropped in related WebConfig of IIS. 

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <cors enabled="true" failUnlistedOrigins="true">
            <!-- Below is the list of URLs to be documented for allowing the Cross Origin Resource Sharing to be enabled -->
            <add origin="https://daraz.com" />
            <add origin="https://microsoft.com"
                 allowCredentials="true"
                 maxAge="120"> 
                <allowHeaders allowAllRequestedHeaders="true">
                    <!-- Below is the list of custom headers to be added in CORS to be enabled -->
                    <add header="header1" />
                    <add header="header2" />
                </allowHeaders>
                <allowMethods>
                     <add method="POST" />
                     <add method="GET" />
                     <add method="OPTIONS" />
                </allowMethods>
                <exposeHeaders>
                    <add header="header1" />
                    <add header="header2" />
                </exposeHeaders>
            </add>
            <add origin="http://*" allowed="false" />
        </cors>
    </system.webServer>
</configuration>
                                                  
```
References, Reading Material is here. 

https://portswigger.net/web-security/cors
https://portswigger.net/web-security/cors/access-control-allow-origin
https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
https://www.w3.org/wiki/CORS_Enabled#For_IIS7
https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
https://docs.microsoft.com/en-us/iis/extensions/cors-module/cors-module-configuration-reference

                                                  
                                                  
## Microsoft IIS Tilde Vulnerability
This is a very old vulenrability and detailed write up is documented at https://support.detectify.com/support/solutions/articles/48001048944-microsoft-iis-tilde-vulnerability             
The remidiation is as follows
                                                  
1. Discard all web requests using the tilde character 
1. Open Registry editor by doing START > Run, followed by "RegEdit".
2. add a registry key named **NtfsDisable8dot3NameCreation** to **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem**. 
3. Set the value of the key to 1 to mitigate all 8.3 name conventions on the server.
