---
layout: default
title: Lost my SharePoint Farm Passphrase
---
# Lost my SharePoint Farm Passphrase

While adding a server to a client's farm we made the discovery that they had lost track of their farm passphrase. 

### What now?

No biggie, [Set-SPPassPhrase](https://docs.microsoft.com/en-us/powershell/module/sharepoint-server/set-sppassphrase?view=sharepoint-ps) to the rescue, right? 

I threw down one of these and expected I'd be good to go:

```PowerShell
$passphrase = ConvertTo-SecureString -string "y0L0$wag" -asPlainText -Force
Set-SPPassPhrase -PassPhrase $passphrase
```

When it came time to add the new server to the farm, I got one of these for my trouble:

```
Failed to connect to the configuration database.

An exception of type System.ArgumentException was thrown. Additional exception information: Error during decryption. Ensure the passphrase is correct.
```
I knew the passphrase was correct, you don't forget a passphrase like `y0L0$wag` (_like_ `y0L0$wag`, not actually `y0L0$wag` but equally memorable). 

The culprit: cache.

### Clear your configuration cache

I adapted this [Clear-SPConfigCache](https://gallery.technet.microsoft.com/office/Clear-Configuration-Cache-d55bcba9) script to flush the configuration cache on all core SharePoint Servers. You don't need to worry about SQL, SharePoint Add-In hosts or Office Online Servers. Once complete, I was back on my merry way, adding servers left, right and centre.
