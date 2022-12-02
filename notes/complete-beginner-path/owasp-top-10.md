# OWASP top 10 

## Severity 1 

### Injection

Common injection attack examples:

- SQL injection -> user input passed to SQL queries allowing manipulation
- Command injection -> input is passed to sytem commands allowing arbitrary system command execution

After the attacker successfully passes input and it is interpreted correctly they can:

- Access, modify and delete information in a database e.g steal credentials or personal details
- Execute arbitrary system commands on a server allowing an attacker to gain access to users' systems

Preventative measures:

- Use a whitelist/allow list, rejecting unsafe input
- Stripping input, removing the dangerous characters before they're processed

### Command Injection

Command injection occurs when server-side code in a web app makes a system call on the hosting machine. It's a vulnerability that allows an attacker to take advantage of the system call to execute OS commands on the server

### Command Injection Practical

What strange text file is in the website root directory? 
- drpepper.txt

What user is this app running as?
- www-data

What's the users' shell set as?
- /usr/sbin/nologin

What version of Ubuntu is running?
- 18.04.4

Print out the MOTD, what favourite beverage is mentioned?
- Dr Pepper

## Severity 2

### Broken Authentication (now Identification and Authentication Failures)

If an attacker finds flaws in an authentication mechanism they can exploit them to gain access to other users' accounts and sensitive data.

Common flaws in authentication mechanisms are:

- Brute force attacks: Launch brute force attacks using multiple auth attempts to guess username password combinations
- Use of weak credentials: Without strong password policies passwords can be guessed easily without brute forcing and without multiple attempts
- Weak session cookies: If session cookies contain predictable values, an atacker can set their own cookies and access other users' accounts

Mitigation depending on the flaw:

- To avoid brute forcing, set an automatic lockout after a certain number of failed attempts
- To avoid guessing of passwords, set a strong password policy
- Implement MFA, making accessing credentials more difficult

### Broken Authentication Practical

What is the flag found in darren's account?
- fe86079416a21a3c99937fea8874b667

What is the flag found in arthur's account? 
- d9ac0f7db4fda460ac3edeb75d75e16e

## Severity 3

### Sensitive Data Exposure (now Cryptographic Failure)

When a webapp accidentally divulges sensitive data, we refer to it as "Sensitive Data 
Exposure".This is often data directly linked to customers (e.g. names, dates-of-birth,
financial information, etc), but could also be more technical information, such as usernames
and passwords. At more complex levels this often involves techniques such as a "Man in The 
Middle Attack", whereby the attacker would force user connections through a device which 
they control, then take advantage of weak encryption on any transmitted data to gain access 
to the intercepted information (if the data is even encrypted in the first place...). Of 
course, many examples are much simpler, and vulnerabilities can be found in web apps which 
can be exploited without any advanced networking knowledge.


### Sensitive Data Exposure Practical

What is the name of the mentioned directory?
- /assets

Navigate to the directory you found in question one. What file stands out as being likely to
contain sensitive data?
- webapp.db

Use the supporting material to access the sensitive data. What is the password hash of the 
admin user?
- 6eea9b7ef19179a06954edd0f6c05ceb

Crack the hash.
What is the admin's plaintext password?
- qwertyuiop

Login as the admin. What is the flag?
- THM{Yzc2YjdkMjE5N2VjMzNhOTE3NjdiMjdl}

## Severity 4

### XML External Entities (now consolidated with Security Misconfigurations)

XXE attacks abuse features of XML parsers/data. It often allows an attacker to interact 
with any backend or external systems that the application itself can access and can allow 
the attacker to read the file on that system. They can also cause Denial of Service (DoS) 
attack or could use XXE to perform Server-Side Request Forgery (SSRF) inducing the web 
application to make requests to other applications. XXE may even enable port scanning and 
lead to remote code execution.

There are two types of XXE attacks: in-band and out-of-band (OOB-XXE).
1) An in-band XXE attack is the one in which the attacker can receive an immediate response 
to the XXE payload.

2) out-of-band XXE attacks (also called blind XXE), there is no immediate response from the 
web application and attacker has to reflect the output of their XXE payload to some other 
file or their own server.

### eXtensible Markup Language

What is XML?

XML (eXtensible Markup Language) is a markup language that defines a set of rules for 
encoding documents in a format that is both human-readable and machine-readable. It is a 
markup language used for storing and transporting data. 

Why we use XML?

1. XML is platform-independent and programming language independent, thus it can be used on any system and supports the technology change when that happens.

2. The data stored and transported using XML can be changed at any point in time without affecting the data presentation.

3. XML allows validation using DTD and Schema. This validation ensures that the XML document is free from any syntax error.

4. XML simplifies data sharing between various systems because of its platform-independent nature. XML data doesn’t require any conversion when transferred between different systems.

Syntax

Every XML document mostly starts with what is known as XML Prolog.

<?xml version="1.0" encoding="UTF-8"?>


Above the line is called XML prolog and it specifies the XML version and the encoding used 
in the XML document. This line is not compulsory to use but it is considered a 
`good practice` to put that line in all your XML documents.

Every XML document must contain a `ROOT` element. For example:

<?xml version="1.0" encoding="UTF-8"?>
<mail>
   <to>falcon</to>
   <from>feast</from>
   <subject>About XXE</subject>
   <text>Teach about XXE</text>
</mail>


In the above example the <mail> is the ROOT element of that document and <to>, <from>, 
<subject>, <text> are the children elements. If the XML document doesn't have any root 
element then it would be considered `wrong` or `invalid` XML doc.

Another thing to remember is that XML is a case sensitive language. If a tag starts like 
<to> then it has to end by </to> and not by something like </To>(notice the capitalization 
of T)

Like HTML we can use attributes in XML too. The syntax for having attributes is also very 
similar to HTML. For example:
<text category = "message">You need to learn about XXE</text>

In the above example category is the attribute name and message is the attribute value.

### XXE Payload

read /etc/passwd:
```
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
<root>&read;</root>
```

read users ssh keys:
```
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///home/*username*/.ssh/id_rsa'>]>
<root>&read;</root>
```

### XXE Practical

What is the name of the user in /etc/passwd?
- falcon

Where is falcon's SSH key located?
- /home/falcon/.ssh/id_rsa

What are the first 18 characters for falcon's private key
- MIIEogIBAAKCAQEA7b

