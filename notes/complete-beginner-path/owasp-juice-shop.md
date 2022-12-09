### Let's go on an adventure! - Reconnaissance

Navigated to:

Customer Feedback -> submitted a review

About Us -> downloaded terms of use from /ftp/legal.md, saw my review in the feedback 
slideshow

Photo Wall -> photos stored in /assets/public/images/uploads


Question 1: What's the admin's email address?

method: the product reviews show the user's email address

answer: `admin@juice-sh.op`

Question 2: What parameter is used for searching?

method: use the search bar and look at the url change

answer: q

Question 3: What show does Jim reference in his review?

method: Jim's review mentions a replicator, a google search shows it comes from star trek

answer: Star Trek

### Inject the juice - SQL Injection

Question 1: Log into the admin account!

method: send login request through login page with any credentials, intercept with burpsuite 
and change the email to `' or 1=1--`.

`'` closes the query, `or` returns true if either side is true; `1=1` is true and `--` is an 
SQL comment tag, removing any other restrictions from the SQL query

answer: 32a5e0f21372bcc1000a6088b93b458e41f0e02a

Question 2: Log into the bender account!

method: send login request with `bender@juice-sh.op`, intercept with burpsuite and change the
email filled value to `bender@juice-sh.op'--`.

The `'` closes the SQL query and the `--` comments out any other restrictions loggin us in 
as bender

answer: fb364762a3c102b2db932069c0e6b78e738d4066

### Who broke my lock?! - Broken Authentication

Question 1: Brute force the admin account!

method: intercept a login request using the admin email and any password. Send the request 
to the Intruder and replace the password value with `§§`. Load best1150.txt from seclists as 
the payloads and start the attack. The password is `admin123`

answer: c2110d06dc6f81c67cd8099ff0ba601241f1ac0e

Question 2: Reset Jim's password!

method: attempt to reset Jim's password, security question is `Your siblings middle name`. 
Jim mentioned Star Trek in a review so google Jim Star Trek. Wiki page for James T Kirk 
comes up, and his brother's middle name is Samuel. Reset his password using the answer to 
the security question

answer: 094fbc9b48e525150ba97d05b942bbf114987257

### AH! Don't look! - Sensitive Data Exposure

