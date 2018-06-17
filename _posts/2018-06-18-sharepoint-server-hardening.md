---
layout: default
title: SharePoint Server Hardening
excerpt: Some notes on SharePoint Server hardening. I've been applying these to SharePoint Server 2016 but these are IIS-level fixes so as far as I can tell all of this is applicable to SharePoint 2013 (and probably 2010 as well).
---
# SharePoint Server Hardening

Some notes on SharePoint Server hardening. I've been applying these to SharePoint Server 2016 but these are IIS-level fixes so as far as I can tell all of this is applicable to SharePoint 2013 (and probably 2010 as well).

## HTTP-only cookies with secure flags

Whether you like it or not, SharePoint bakes a lot of cookies and doesn't secure them by default, leaving them potentially vulnerable to XSS attacks. Lock down the old cookie jar by ensuring your `web.config` sets the following flags:

```xml
<httpCookies httpOnlyCookies="true" requireSSL="true" />  
```

## Cache management

You may want to go with `no-cache`, `no-store` or both. Keep in mind `no-cache` != `don't cache` and choose accordingly. Consider performance impacts. 

```xml
<add name="Cache-Control" value="no-store" />
```

## Updating the web.config programatically

As a general rule don't want to be hacking away at your `web.config` files manually. Particularly if you're working with a multi-server farm, or virtually any production scenario. That's where the [SPWebConfigModification](https://msdn.microsoft.com/en-us/library/microsoft.sharepoint.administration.spwebconfigmodification.aspx) class comes in. Changes applied to the SPWebApplication`s `WebConfigModifications` are:
* applied to all servers that host that web application
* automatically applied to any new web front-end servers added to your farm
* automatically applied when you extend the SPWebApplication to a server that wasn't previously hosting that SPWebApplication 
* tracked centrally

Here's an example adapted from [Sam Betts' blog post on custom response headers](https://blogs.msdn.microsoft.com/sambetts/2015/07/16/add-custom-http-headers-to-sharepoint-responses/) that will set the `httpOnlyCookies` and `requireSSL` flags.

```powershell
[CmdletBinding()]
Param(
    [Parameter(Mandatory=$true)]
    [String]$TargetWebApp
)

Add-PSSnapin Microsoft.SharePoint.PowerShell -ErrorAction SilentlyContinue

$WebApp = Get-SPWebApplication $TargetWebApp

$configPath = "configuration/system.web"

$configModAccessControl = New-Object Microsoft.SharePoint.Administration.SPWebConfigModification
$configModAccessControl.Path = $configPath
$configModAccessControl.Name = "httpCookies[@httpOnlyCookies='true'][@requireSSL='true']"
$configModAccessControl.Value = "<httpCookies httpOnlyCookies='true' requireSSL='true' />"
$configModAccessControl.Sequence = 0
$configModAccessControl.Owner = ""
$configModAccessControl.Type = 0

if (($WebApp.WebConfigModifications | where-object { $_.Name -eq $configModAccessControl.Name } | measure).Count -eq 0) {
    Write-Host "Adding " $configModAccessControl.Name -ForegroundColor Green
    $WebApp.WebConfigModifications.Add($configModAccessControl)
    $WebApp.Update()
    $WebApp.Parent.ApplyWebConfigModifications()
}

else {
    Write-Host $configModAccessControl.Name already exists. skipping. -ForegroundColor Blue    
}
```

To review an `SPWebApplication`'s web config changes, simply call:

```powershell
$WebApp = Get-SPWebApplication https://mywebapp
$WebApp.WebConfigModifications
```
