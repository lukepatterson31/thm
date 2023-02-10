# Introduction to Web Hacking

### Walking an application

Use view source and developer tools (Inspector, Debugger and Network) to walk an application

### Content Discovery

**Manual**

Robots.txt: `<TARGET>/robots.txt`

Favicon: Get the favicon's md5 value and lookup in the [Favicon DB](https://wiki.owasp.org/index.php/OWASP_favicon_database)

Sitemap.xml: `<TARGET>/sitemap.xml`

HTTP Headers: `curl <TARGET> -v `

Framework Stack: Establish the framework (favicon, comments, copyright notices, etc.) and 
learn more about it from the website and relevant documentation.

**Automated**

ffuf: `ffuf -w <WORDLIST> -u <TARGET>`

dirb: `dirb <TARGET> <WORDLIST>`

Gobuster: `gobuster dir -u <TARGET> -w <WORDLIST>` *interesting commands other than dir*

**OSINT**

Google Dorks: [Google Hacking DB](https://www.exploit-db.com/google-hacking-database)

Wappalyzer: [Wappalyzer](https://www.wappalyzer.com/)

Wayback machine: [The Wayback Machine](https://archive.org/web/)

GitHub: Look for source code

S3 Buckets: `http(s)://<NAME>.s3.amazonaws.com`, Gobuster 

### Subdomain Enumeration

**Bruteforce**

DNS Bruteforce: [dnsrecon repository](https://github.com/darkoperator/dnsrecon)

**OSINT**

SSL/TLS Certificates: Certificate Transparency logs DB search [CRT](http://crt.sh/) & [Entrust CT search](https://ui.ctsearch.entrust.com/ui/ctsearchui)

Search Engines: Use dorks to find subdomains `-site:www.url.com site:*.url.com`

Sublist3r: [Sublist3r repository](https://github.com/aboul3la/Sublist3r)

**Virtual Host**

Host header: Fuzz the Host header `ffuf -w <WORDLIST> -H "Host: FUZZ.url.com" -u <TARGET>`. 
Filter the output with `-fs <SIZE>`

### Authentication Bypass

**Username Enumeration**

ffuf: `ffuf -w <WORDLIST> -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u <TARGET> -mr "username already exists"`
Change `-d` based on required inputs and `-mr` based on response.

**Brute Force**

ffuf: `ffuf -w <USERNAME-WORDLIST>:W1,<PASSWORD-WORDLIST>:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u <TARGET>/login -fc 200`
`-fc` filters by HTTP status code.

**Logic Flaw**

Flaw in the code that allows an unauthenticated user to bypass authentication checks.
Capture password reset and try another account, PHP code using === meaning different casing
will bypass, etc.

**Cookie Tampering**

Plain text cookies can be edited directly, base64 cookies can be decoded and hashed ones 
need identifying and cracking.

### IDOR

Decode or crack the ID then tamper and encode or hash it. Use 2 accounts and swap the ID 
numbers between them to see if IDOR is present.

Check the networks dev tool when the page loads and look for endpoint calls passing ID's.

### File Inclusion


