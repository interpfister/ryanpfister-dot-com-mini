---
path: '/2008/04/guide-to-making-a-personal-web-site-with-a-penn-state-focus/'
date: '2008-04-10'
title: 'Guide to making a personal Web site (with a Penn State focus)'
---

If you’re a Penn State student (or anyone else) who wants a spot on the Internet, here’s my getting starting guide based on my own experiences.

Topics covered:

- Hosting your site (where to save your files)
- Creating and updating files
- To dot.com or not to dot.com (or .org, or .net)

# Hosting your site

For people to be able to view your site on the Internet, you need to put your files on a Web server. If you’re at Penn State, the university provides some free space for you to use. If you’re not (or you’ll be graduating soon — your PSU space ends 6 months after graduation), I recommend some free and cheap alternatives at the bottom of this section.

## At Penn State

1. First you have to apply for Web space at https://www.work.psu.edu/webspace/ The quiz questions are pretty obvious. It takes a few days to set up.

2. Now you need to open the folder Penn State created for you to store your files. On Windows, Open My Network Places from your desktop. Double-click “Add Network Place.” Type in: \\win.pass.psu.edu\xyz123 (replace xyz123 with your user id). Hit next and type in your username and password.

- NOTE: If you’re off-campus, you probably won’t be able to access your files directly like this. Instead, you’ll have to use SSH to upload your files (see below).

3. Your Penn State file space (Penn State calls it the “PASS space”) will show up. Open the www folder. This is where you will store all your Web site files.

Now you’re ready to go. Jump down to the section on creating and updating files. They will appear at http://www.personal.psu.edu/xyz123 (again, replacing xyz123 with your user name).

## Not at Penn State

If you’re not at Penn State, you’ll need to find a place to host your files. Free Web hosts abound, but the only one I’ve used is Yahoo! Geocities. It puts an annoying ad on your page, but the service has been around for a while so it’s probably pretty stable. You’ll also have to manually upload your files using their Web interface.

As an alternative to free Web hosts, I recommend “pay as you go” Web hosting from NearlyFreeSpeech.NET. You make an initial deposit and then they just draw against it. Fees are about \$1 a gigabyte of transfer, so even five dollars could last you for years. I use this service for this Web site and I’ve had a great experience. The one downside is they have no Web-based tools for uploading files, so you’ll have to learn to use SSH.

## Using SSH

If you can’t access your Web space using the “My Network Places” method above, you’ll have to use SSH to upload your files to your server. I recommend WinSCP for Windows users. Download the standalone application.

To get files on your Web space, open the program and type in the server (ftp.personal.psu.edu for Penn State) and your user name and password. Click connect, accept the encryption key and your files should show up. Then just drag files from your hard drive to the Web server directory. If you need to make changes to files, just re-upload them and overwrite the old files.

Note that unlike with the “My Network Places” method, you won’t be able to edit your files directly on the server. Instead, you’ll have to edit them on your hard drive and then upload them.

# Creating and updating files

Download my basic template to get started. You can use your own files if you want, but I’m going to reference the template specifically.

index.html is the main file.
works.html is where the list of stories is.
site.css is the stylesheet that controls how the pages look.
the png file is the mug shot that appears on the right side of the page.
Just edit these files and drag them to your Web space. Also upload any other files you referenced. I recommend using a text editor to edit your files, but if you use a graphical editor, make sure your links are relative (e.g. don’t start with file://). Otherwise they won’t work on the Web site.

For an introduction to HTML and CSS editing, check out HTML Dog. I’ll write a more specific tutorial later if I get around to it.

# To dot.com or not to dot.com

Domains are cool. The address looks impressive on your resumé, you get every address @yourname.com and your url is a lot easier to remember.

You get a domain by registering it with a company called a registrar. My current favorite is 1&1 (which is also a hosting company). I like it because it’s reliable and its prices are on the cheaper end. Dot.com domains are \$7/year; less common suffixes like .mobi are sometimes less. The company will keep your credit/debit card and charge you each year. Another perk of 1&1 is that they offer private registration, so you don’t have to reveal your contact information on domain name registry listings.

Once you’ve registered your domain, you have two options. The first is to forward your domain to the url where your files are hosted. If you take this route, you have two options: masked forward or redirect. Under the first option, your domain stays in the address bar but the file inside come from your Web host. Under the second option, the real URL of your Web site appears in the address bar.

If you care about your domain staying in the address bar, pick the first option. Just keep in mind that urls of pages won’t show up in the address bar. So if someone likes your “works” page and they copy and paste the url, it will be the one for your main page, not the works page specifically. Of course if you have a Web site with just a few pages it’s probably not a big issue.

The second route for forwarding your domain is to use DNS. This works with paid Web hosts like NearlyFreeSpeech.NET. This method will keep your domain url in the address bar and let you type in specific urls. It takes a little doing to set up (maybe I’ll write up a tutorial later) but basically you need to have 1&1 forward your name servers or DNS to your Web host.
