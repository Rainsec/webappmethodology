# Web-App Methodology
My 5-Step methodology for enumerating Web-Apps, aimed at OSCP students/CTF players

***************

Begin with > ` nmap -sV -A -p- -T4 -Pn <IpAddress> --max-retries=1 -oA nmap ` 

Look at all ports. If SSH is there, leave that till the end dont even bat an eye at the version. 
`(I work my way from Top to Bottom i.e. typically begin with FTP port 21)`

If 21 FTP is up -  searchsploit the version. Try anon login with `ftp:ftp` and grab all files possible for further inspection

If HTTP/HTTPS is open, I run Nikto on them real quick - THIS is purely out of old habit, but I still kept it going for sake of following strict methodology

I then run ffuf (`or gobuster if you like`) on the http/s ports with  .txt,.php extensions first on the medium2-3 wordlist from dirbuster directory. If im dealing with an IIS server, I'll add .asp,.aspx to the extensions

If any other service is up like SMB, I'll run a quick searchsploit on it's version. I'll run smbmap -H <targetIP> -R. I then also run smbclient -L <targetIP> and see whats up.

For SMB -  you want as much info as possible, so any version numbers I'll go and grab it and run it through google. (`you could use msf auxillary module to find the version, but it's limited to ONE use on the exam so dont waste that usage on a version check`)        *there's also a crazy method me and my buddy figured out to find smb versions when nothing is working to find it, this will be added in our final guidebook*

For any other port open, the same method applies. If there's a version on it, I'm googling / searchsploiting it. If no version but I have service name, I'm googling that service and reading up on it and how to exploit. Typical search parameters in Google are as basic as "<serviceName> exploit" and see what unfolds...



That's what I do for nmap. When it comes to hacking at web-apps (`I used to be OK at this until my buddy taught me the ways of patient enumeration`) you want to create a flow of enumeration and always stick to it. Always stick to it even when things are looking rough and you're making no progress, believe me I've hit brick walls sometimes but having trust in the methodology made go back to step 1 and do everything again - always works 


## **The Web-App Enumeration Flow:**

************

1. Visit the page, have a browse around. Don't think about hacking stuff yet. Check it out like it's a bounty program, click all links and tabs see how the web-app reacts. Simply browse. I should mention before browsing - leave a dirbusting tool running in the background like ffuf or gobuster with appropriate extensions!

2. View page-source and check for comments. If it's a CTF -  you could possibly find comments that mean something of value i.e hidden directory or credentials.

3. Re-visit the page but this time check where each link on the page takes you to. Take note of the URL, hovering over the link and looking bottom left of screen usually brings up what URL you'll be taken to. This stage is where you want to basically re-visit the page but with the intention of looking for something *interesting*. Be curious and check all things. Read the blog if the site has a blog, keep an eye for usernames i.e Authors. Pay very close attention to detail now, you're finally going into the mindset of a Hacker as opposed to a regular customer browsing the site. Look at login panels, try default credentials, try basic SQLi such as ` ' or 1=1-- - `. Try creating an account to see if your privileges increase in anyway. Change parameters in the URL incase you reveal a hidden entry or find some form of a LFI or a SQLi i.e http://target/cms/page?id=1 > http://target/cms/page?id=9 > http://target/cms/page?id=/etc/passwd.   These are things you want to experiment with - you want to see any sort of different reaction from the application. Error messages can infact be **GOOD INFORMATION!** 

4. Check your ffuf/gobuster scans and start to visit these endpoints. If you've visited them and found nothing, re-scan those endpoints further i.e. say for example you find http://target/cms and nothing is there, you should alter your ffuf tool to FUZZ further > http://target/cms/FUZZ  with your extensions. This is the stage where you wanna be even MORE curious and keep scanning those endpoints, you do NOT want to miss anything out of laziness, trust me it'll bite you down the line and it'll cost you time. Make sure you have **e n u m e r a t e d** ALL of the endpoints, I can't stress enough how important this is!!! Many times I've wasted countless hours out of laziness of re-scanning... 


5. This is the stage where you want to think about what you're facing and how you're going to exploit it. Are you facing some LFI? Go onto GitHub and look at the LFI cheatsheet on [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion) or simply Google LFI exploits and follow some guys blog. Are you against a CMS? Google and see exploits for it and give them ALL a shot. Some RFI but it's only grabbing index.php off your server instead of shell.php? Change your shell name to index.php and watch the magic unfold. Up against File Upload? tons of ways to bypass with magic bytes and such. Check ippsec.rocks for file upload bypasses, easy win! Stay curious as hell at this point. Think outside the box. Spin up Burp Suite if needed, capture the request and send it to Repeater and constatly mess around with parameters of whatvever you're trying to pull a Reverse Shell from. Whether it's some Command Injection, Directory Traversal or an LFI just ensure you're throwing loads of payloads at it! There's repositories out there for these kinds of tasks - give them a try. Remember to URL-encode all payloads requests too with Repeater! Maybe you're stuck on getting a Linux reverse shell - go to pentestmonkeys on Google and look at their Linux reverse shell one-liners. I guarantee one of them will work :D





Honeslty, once you've reached Step 5 and you're stuck it just means you can't pull off the exploit to gain a reverse shell. Every scenario is different so I can't tell you exactly what to do here -  but you gotta think logical or sometimes out the box for this section. 

It could either be you need some exploit from GitHub or you need burp intercept to send it to repeater and work on Command Execution to get a shell. *keep in mind to always URL-encode your parameters in Repeater otherwise sometimes they don't work* 

Remember for Step 5 to think LOGICAL of how you can leverage a reverse shell. If you're still stuck and see nothing of interest, you have missed crucial information somewhere. Maybe you have credentials and they aren't logging you into the Web-App - well remember you had FTP,SSH and SMB open?! Fly the creds over there!!!

Rememeber, you're dealing with an application that was *MADE* to be exploited. It's vulnerable somehow and you need to add up the dots



***Happy Hacking & Try SMARTER.***

