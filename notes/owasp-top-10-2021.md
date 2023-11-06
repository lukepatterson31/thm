# OWASP Top 10 - 2021

1. Broken Access Control
2. Cryptographic Failures
3. Injection
4. Insecure Design
5. Security Misconfigurations
6. Vulnerable and Outdated Components
7. Identification and Authentication Failures
8. Software and Data Integrity Failures
9. Security Logging and Monitoring Failures
10. Server-Side Request Forgery (SSRF)


### 1. Broken Access Control

Access controls prevent users accessing content they are not authorized to access, Broken Access Control 
allows users to bypass authorisation.

YouTube private video access example: https://bugs.xdavidhu.me/google/2021/01/11/stealing-your-private-videos-one-frame-at-a-time/

**IDOR Challenge**

IDOR or Insecure Direct Object Reference refers to an access control vulnerability where you can access 
resources you wouldn't ordinarily be able to see. This occurs when the programmer exposes a Direct Object 
Reference, which is just an identifier that refers to specific objects within the server. Direct object 
references aren't the problem, lack of validation in the application is.

In the challenge the `note_id` parameter is vulnerable to IDOR

![IDOR](./pictures/owasp-top-10-2021-idor.png)

By changing the value of `note_id` we can see other users notes

![IDOR](./pictures/owasp-top-10-2021-idor-flag.png)

### 2. Cryptographic Failures

Misuse or lack of cryptographic algorithms for protecting sensitive information. Encrypting data in transit
prevents eavesdropping of network packets, encrypting data at rest prevents data stored in servers from being
read.

Examples of cryptographic failures:
- accidental sensitive data exposure in a web app
- weak encryption on data intercepted in Man in The Middle Attacks or data breaches

**Cryptographic Failure Challenge**

The login.php source contains a comment mentioning the app DB is stored in /assets

![Crypto Fail](./pictures/owasp-top-10-2021-crypto-fails-db.png)

We can download and view the users table with sqlite3 giving us all usernames and password hashes

![Crypto Fail](./pictures/owasp-top-10-2021-crypto-fails-db-content.png)

Using crackstation we can crack 2 of the 3 hashes, one of them is the admin user's password

![Crypto Fail](./pictures/owasp-top-10-2021-crypto-fails-hashes.png)

Logging in as admin gives us the flag

![Crypto Fail](./pictures/owasp-top-10-2021-crypto-fails-flag.png)

### 3. Injection

Injection flaws happen when user input is used for parameters or commands within the application.
Injection attacks depend on what technologies are used and how these technologies interpret the input.

Some common examples include:

- SQL Injection: This occurs when user-controlled input is passed to SQL queries. An attacker can pass in 
SQL queries to manipulate the outcome of such queries. This could potentially allow the attacker to access, 
modify and delete information in a database when this input is passed into database queries.

- Command Injection: This occurs when user input is passed to system commands. An attacker can execute 
arbitrary system commands on application servers.

The main defence for preventing injection attacks is ensuring that user-controlled input is not interpreted 
as queries or commands.

- Using an allow list: Input is compared to a list of safe inputs or characters. If the input is safe, then 
it is processed. Otherwise it's rejected by the application.
- Stripping input: If the input contains dangerous characters, they are removed before processing.

**Command Injection**

Using Cowsay online we can execute inline commands `$(command)` on the target

![Command Injection](./pictures/owasp-top-10-2021-command-injection.png)

### 4. Insecure Design

Insecure design refers to vulnerabilities which are inherent to the application's architecture. They are not 
vulnerabilities regarding bad implementations or configurations, but the idea behind the whole application 
(or a part of it) is flawed from the start.

Insecure Password Reset example: https://thezerohack.com/hack-any-instagram

**Insecure password Reset Practical**

We can reset a user's password by answering security questions

There is no limit on the number of tries to answer a given question so we can bruteforce the "What is your 
favourite colour?" question to reset joseph's password and obtain the flag

### 5. Security Misconfiguration

Security misconfigurations are a lack of appropriate configuration.

- Poorly configured permissions on cloud services (S3 buckets)
- Having unnecessary features enabled, like services, pages, accounts or privileges
- Default accounts and passwords
- Error messages that are overly detailed and allow the attacker to find out more about the system
- Not using HTTP security headers

**Debugging Interfaces**

A common security misconfiguration concerns the exposure of debugging features in production software. 

Werkzeug panel in [Patreon 2015 hack](https://labs.detectify.com/2015/10/02/how-patreon-got-hacked-publicly-exposed-werkzeug-debugger/)

**Practical Example**

The target has an exposed Werkzeug debugging console, allowing code execution on the server

![Debugging Interface](./pictures/owasp-top-10-2021-security-misconfiguration-console.png)

We can view the source code to grab the flag

![Debugging Interface](./pictures/owasp-top-10-2021-security-misconfiguration-flag.png)

### 6. Vulnerable and Outdated Components

Vulnerabilities for older versions of software that are well known, and easily exploitable as a result.

**Vulnerable and Outdated Components Lab**

The target app is vulnerable to SQLi in the password parameter in the admin login page

![SQLi](./pictures/owasp-top-10-2021-vulnerable-components-admin-sqli.png)

Admin Book List

![admin access](./pictures/owasp-top-10-2021-vulnerable-components-admin-book.png)

Using the /admin_add.php page we can upload a PHP webshell

![shell upload](./pictures/owasp-top-10-2021-vulnerable-components-upload-shell.png)

Set cmd to cat and get the flag

![Flag](./pictures/owasp-top-10-2021-vulnerable-components-flag.png)

### 7. Identification and Authentication Failures

Common flaws in authentication mechanisms:

- Brute force attacks: If a web application uses usernames and passwords, an attacker can try to launch 
brute force attacks that allow them to guess the username and passwords using multiple authentication 
attempts.  
- Use of weak credentials: Web applications should set strong password policies.  
- Weak Session Cookies: Session cookies are how the server keeps track of users. If session cookies contain 
predictable values, attackers can set their own session cookies and access users' accounts.  

Mitigations for broken authentication mechanisms:

- To avoid password-guessing attacks, ensure the application enforces a strong password policy.  
- To avoid brute force attacks, ensure that the application enforces an automatic lockout after a certain 
number of attempts.  
- Implement Multi-Factor Authentication.  

**Identification and Authentication Failures Practical**

Using user re-registration to exploit a flaw in the authentication mechanism

Register as darren adding a space in front, then login with the newly created password
![register](./pictures/owasp-top-10-2021-id-and-auth-failure-register-darren.png)
![login](./pictures/owasp-top-10-2021-id-and-auth-failure-darren.png)

Register as arthur adding a space in front, then login with the newly created password
![register](./pictures/owasp-top-10-2021-id-and-auth-failure-register-arthur.png)
![login](./pictures/owasp-top-10-2021-id-and-auth-failure-arthur.png)

### 8. Software and Data Integrity Failures

