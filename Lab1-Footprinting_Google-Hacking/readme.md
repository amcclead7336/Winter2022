# Footprinting: Google Hacking
## Introduction
The internet is massive today but it wasn't always so. In the early days, someone would give you their URL or IP address and you'd just plug that into your address bar and go straight to their site. The internet wasn't the staple that it is today.

What made the internet a necessity was search engines. Search engines enabled average users to find new websites without having to know their exact URLs. Search engines would go out and find addresses, rate them, and store them. This is how queries can be so accurate and fast at the same time.

Something most people don't realize is how extensively Search Engines crawl sites. This often leads to sensitive information being exposed unintentionally. Even though the information is crawled by Search Engines, this doesn't mean they come up in a typical search.

A Google Hack, or Dork as it's often called, is the application of advanced search criteria to find these sensitive files and sites. A good site for researching Dorks is https://www.exploit-db.com/google-hacking-database


## Basics:
Google has many powerful advanced operators and most are easily readable. Some of my favorites are:

* inurl:
* site: 
* intext:

There are other many more operators including "-" for excluding results. example: -keyword. Using "()" for segmenting queries. We'll see examples next. The advance searching language is very flexible as we'll see so feel free to explore and create your own searches.


## Applications:
### Useful daily application:
Some sites are designed for administrative use and are not intended to be used by the general public. This doesn't stop Web Crawlers from collecting them, however, they don't usually show in search results.

General Search: [northwestern sps courses](https://www.google.com/search?q=northwestern%20sps%20courses) <br>
Results:
https://sps.northwestern.edu/part-time-undergraduate/view-all-courses.php

Advanced Search: [inurl: (sps & northwestern & view courses)](https://www.google.com/search?q=inurl:%20(sps%20%26%20northwestern%20%26%20view%20courses)) <br>
Results: https://sps.northwestern.edu/part-time-undergraduate/view-courses.php

#### Alternative advanced search
Jumping from page to page and using a site's limited search tools can be frustrating. You can use Google's advanced search parameters to search websites faster and more efficiently.

Advanced Search: [site:linkedin.com intext:("Northwestern University" & "Experience" & "Activity")](https://www.google.com/search?q=site:linkedin.com%20intext:(%22Northwestern%20University%22%20%26%20%22Experience%22%20%26%20%22Activity%22))

***
### Exposed IoT Devices
As the Internet of Things (IoT) expands, more devices become interconnected. Improperly configured devices can be exposed on the internet. Web cameras without authentication can be easily accessed and viewed.

Advanced Search: [intitle:"webcamXP" inurl:8080](https://www.google.com/search?q=intitle:%22webcamXP%22%20inurl:8080)

***
### Unintended access to services
Unintended exposure to login pages is a juicy target for potential hackers. If a max tries policy is not enforced, it's just a matter of time till someone breaks in. FTP servers and Remote Desktop access can be accessed as well.

#### Login pages:
[inurl *:8080/login.php](https://www.google.com/search?q=inurl%20*:8080/login.php)

#### Ftp Server Access:
[intitle:"index of" inurl:ftp](https://www.google.com/search?q=intitle:%22index%20of%22%20inurl:ftp)

#### Remote Desktop Protocol:
[allinurl:tsweb/default.htm](https://www.google.com/search?q=allinurl:tsweb/default.htm)


***
### Sensitive data exposure
Below are some examples of how data can be crawled by search engines and found by attackers. This data can be log files, environment variables, and even security reports.

#### Username Collection:
[allintext:username filetype:log](https://www.google.com/search?q=allintext:username%20filetype:log)

#### Environment vars:
[db_password filetype:env](https://www.google.com/search?q=db_password%20filetype:env)

#### Report exposure:
[intitle:"report" ("qualys" | "acunetix" | "nessus" | "netsparker" | "nmap") filetype:html](https://www.google.com/search?q=intitle:%22report%22%20(%22qualys%22%20|%20%22acunetix%22%20|%20%22nessus%22%20|%20%22netsparker%22%20|%20%22nmap%22)%20filetype:html)


***
## Summary
Google hacking or Dorking should be in every security professional's repertoire. The tools are tested and proven. They're a part of a tested brand and will continue to grow and update as Technology continues to grow. There is also an expansive and powerful database of dorks to pull from and they're easy to read and customize.

It is vital for blue team members to know what their attack surface is and what information is being exposed to the internet. Using these tools you can see the internet as attackers do and begin to close up insecurities.
