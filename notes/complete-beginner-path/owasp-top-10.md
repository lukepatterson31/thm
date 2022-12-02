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