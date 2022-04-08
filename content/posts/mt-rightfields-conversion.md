---
path: '/2008/05/converting-right-fields-when-upgrading-mt-3-to-mt-4/'
date: '2008-05-06'
title: 'Converting Right Fields when upgrading MT 3 to MT 4'
---

Last week, the incoming Daily Collegian Web editor and I put in some very late hours upgrading our publishing system from Movable Type 3.3 to MT 4. We had a few hiccups; I’ll document them here in case any one else runs into the same thing.

**Why we upgraded:** MT 3.3 was getting slowwwwww. And worse, it looked like it was getting slower the more entries we added — indicating the problem might get worse with time. I have a hunch that MT wasn’t writing the SQL queries correctly to properly leverage indexes. When I installed MT 4, our entry build times were cut almost in half.

The main reason we didn’t upgrade earlier was that on MT 3.3, we were using RightFields, which hasn’t been (and from what it looks like, might never be) upgraded for MT4. I’ve been waiting for it since September and it hasn’t happened. I recently discovered, however, that you can convert RightFields data to CustomFields, which is available for MT4. More on the conversion process later.

**How to get MT4 with CustomFields:** MT 4 is open source, so we thought it wouldn’t matter that our paid support contract from MT 3.3 was almost up. Wrong. Turns out that the “CustomFields” plugin is only available with the “professional pack” (i.e. paid) version. Seems to me that an open source version of Custom Fields is definitely needed.

So I poked around the MT Web site and found out that the education license that we have can be upgraded for free to MT 4 as long as we have a current support contract. No word on how exactly to obtain that upgrade. So I searched around again to find the account info from when we bought MT last May. I logged onto that system and filed a support ticket. At first they told me I needed an upgraded support contract, but after I complained they added it to our account for free.

**Doing the RightFields conversion:** We were storing our RightFields data in an SQL table. The only documentation I could find about migrating to CustomFields was a blog entry and accompanying script called RF2CF. The plugin installed and showed up fine in MT 4, but it didn’t work. I think this is because the plugin was designed for RightFields that was using PluginData instead of an SQL table.

**The hack:** I took out most of the code from RF2CF and wrote code to manually connect to the database. I’ve asked Chad Everett, who wrote RF2CF, for permission to post my changes. (UPDATE: I got permission. Check out the code.) One problem I had was that custom fields apparently wants each blog to have its own set of fields, so you can’t reuse extra fields from one blog to the next.

**The results:** While build times are much faster, MT itself is running a bit slower. Loading pages like the “New Entry” page now takes longer than it used to. Not sure why this is; perhaps a Windows thing. I tried installing MT4 on a FreeBSD server and it’s speeding along just fine.

Leave a comment if you’ve had any similar issues or you have questions about what I did.

**Side note:** PluginData is really annoying. If CustomFields used SQL data too, I could have written a simple script — or even used an Excel Spreadsheet — to do the conversion. No MT knowledge required. But since PluginData is stored in binary, I pretty much have to use MT’s code.
