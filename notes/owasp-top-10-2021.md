# OWASP Top 10 - 2021

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
