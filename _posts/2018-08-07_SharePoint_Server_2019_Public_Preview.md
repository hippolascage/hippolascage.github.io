---
layout: default
title: SharePoint Server 2019 Public Preview
excerpt: Reflections on the SharePoint Server 2019 Public Preview
---

# SharePoint 2019

Bill Baer recently announced the release of the [SharePoint 2019 Public Preview](https://techcommunity.microsoft.com/t5/Microsoft-SharePoint-Blog/Announcing-Availability-of-SharePoint-Server-2019-Preview/ba-p/214427).

The focus appears to be mostly about bringing the Modern Experience from SharePoint Online to SharePoint Server. This means things like Team Sites, Communication Sites, Modern Pages and Modern Lists that you've grown to love (or hate!) in SharePoint Online are now available to on-premises clients.

The GA release is expected in the second half of 2018.

### What's in?

* Modern User Experience
* "Minrole v2" Architecture, which I can't find any more information about - I imagine this is an incremental improvement (think iPhone 8s)
* File size upload limit increased to 15GB (but why would you??)

### What's out?

* PowerPivot Gallery (removed)
* A small part of the Lists SOAP service (deprecated only, the SOAP services are still shipping with the product)
* SharePoint Designer (deprecated but continued support for SharePoint Designer 2013 until 2026)
* InfoPath (deprecated only, still supported)

These are just some things that jumped out at me, not an exhaustive list. 

### Thoughts

InfoPath, while deprecated, still lives. I can't say I'm particularly pleased about that. While Microsoft hasn't provided a suitable replacement, I don't believe that's justification enough for InfoPath to still be the default (or even in the mix) for forms development. Even if it still has a place, the use case has narrowed considerably. Realistically, its use should be limited to building very basic forms where the value in being able to deliver them very quickly outweighs the limitations, restrictions and general obsolescence. 

For example, I visited a client recently who wanted some help rescuing a project. A contractor had convinced them InfoPath was the right move for a non-trivial data capture solution (in 2018!). Unfortunately, they have run into some well-known and predictable issues. No one should have to live like that in 2018.

Instead of turning this into an InfoPath beat up, I'm going to wrap it up here. I'm excited that the Modern Experience is coming to SharePoint Server, so my on-premises clients can now access some of the latest and greatest from SharePoint Online.

![](/assets/2018-08-07/infopath.png)
