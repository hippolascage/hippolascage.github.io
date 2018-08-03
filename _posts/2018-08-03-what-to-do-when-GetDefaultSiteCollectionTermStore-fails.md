---
layout: default
title: What to do when GetDefaultSiteCollectionTermStore fails
excerpt: 
---
# What to do when GetDefaultSiteCollectionTermStore fails

### The Problem

I was in the kitchen whipping up a fresh batch of SharePoint 2016 development environments recently. Using [Export-PnPTermGroupToXml](https://docs.microsoft.com/en-us/powershell/module/sharepoint-pnp/export-pnptermgrouptoxml), I generated some exports from a tasty Taxonomy I had in another environment. Once my deployment scripts had run, I tried to import the Term Sets using [Import-PnPTermGroupFromXml
](https://docs.microsoft.com/en-us/powershell/module/sharepoint-pnp/import-pnptermgroupfromxml) only to have SharePoint Server serve me up a steaming pile of `nope nuggets`:

![](/assets/2018-08-03/getdefaultsitecollectiontermstore.png)

Apparently PnP was unable to establish a TaxonomySession. As I chewed on this error message for a minute, it started to taste familiar.  Depending on how you're interacting with the Managed Metadata Service, you'll get various permutations of errors about `GetTaxonomySession` returning `null` or default Term Stores not being found if the Managed Metadata Service is not configured correctly.

I navigated over to the Managed Metadata Service Proxy's configuration and found that the Service Application Proxy was a little undercooked:

![](/assets/2018-08-03/mmssettings.png)

### The Solution 

`This service application is the default storage location for Keywords` and `This service application is the default storage location for column specific term sets` are both key ingredients unless you have multiple Managed Metadata Service Applications. Tick those boxes and you're good to go.

### The Recipe for Success

The above will work in a bind, but I was only in this predicament because my new build script was missing key ingredients. To make sure your Service Applications are al dente every time, you should configure them programatically as part of your deployment. 

You can bake this into your build scripts by making a call to [Set-SPMetadataServiceApplicationProxy](https://docs.microsoft.com/en-us/powershell/module/sharepoint-server/set-spmetadataserviceapplicationproxy) with a pinch of `-DefaultKeywordTaxonomy` and `-DefaultProxyGroup`. Here's one I prepared earlier:

```PowerShell
Set-SPMetadataServiceApplicationProxy -Identity "Managed Metadata Service Application Proxy" -DefaultProxyGroup -DefaultKeywordTaxonomy
```


Bon appétit!
