# Cryptography

## Hashing - Crypto 101

### What is a hash function?

A hash function is a mathematical function performed on a piece of data of any size that 
transforms it into a hash of a fixed size. It is nearly impossible to go from output to
input

### What is a hash collision?

Hash collision is when 2 different inputs give the same output. Due to the pigeonhole 
effect, collisions are not avoidable. The pigeonhole effect is basically, there are a set 
number of different output values for the hash function, but you can give it any size 
input. As there are more inputs than outputs, some of the inputs must give the same output. 
If you have 128 pigeons and 96 pigeonholes, some of the pigeons are going to have to share.

### Uses for hashing

2 main purposes in Cybersecurity:
1. Verify integrity of data
2. Storing passwords

Instead of storing passwords we can store hashes instead, when we enter our password it is 
hashed and compared with the stored value.

Salting a hash is a way of protecting against duplicate passwords and Rainbow tables. Each 
user is given a unique salt that is added to the start or end of the password before hashing


### Recognising password hashes

Unix style password hashes use a prefix, the standard format is `$format$rounds$salt$hash`

Windows passwords use NTLM, a variant of md4. They're visually identical to md4 and md5 
hashes.

On Linux hashes are stored in /etc/shadow and on Windows they're stored in the SAM

Unix style password prefixes:

Prefix : Algorithm
`$1$` : md5crypt, used in Cisco stuff and older Linux/Unix systems
`$2$, $2a$, $2b$, $2x$, $2y$` : Bcrypt (Popular for web applications)
`$6$` : sha512crypt (Default for most Linux/Unix systems)

Example hashes here:
[Hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)

### Password cracking

Use GPU vs CPU, better equipped although some hashing algorithms (bcrypt) are designed so 
that hashing on a GPU is about the same speed as on CPU

Crackstation, hashes.com, hashcat and John the ripper are hash cracking tools

### Hashing for integrity checking


Integrity Checking

Hashing can be used to check that files haven't been changed. If you put the same data in, 
you always get the same data out. If even a single bit changes, the hash will change a lot. 
This means you can use it to check that files haven't been modified or to make sure that 
they have downloaded correctly. You can also use hashing to find duplicate files, if two 
pictures have the same hash then they are the same picture.

HMACs

HMAC is a method of using a cryptographic hashing function to verify the authenticity and 
integrity of data. The TryHackMe VPN uses HMAC-SHA512 for message authentication, which you 
can see in the terminal output. A HMAC can be used to ensure that the person who created 
the HMAC is who they say they are (authenticity), and that the message hasn’t been modified 
or corrupted (integrity). They use a secret key, and a hashing algorithm in order to 
produce a hash.

## John The Ripper

### Cracking basic hashes

Supply the format to john after identification of hash type with hash-identifier or 
https://hashes.com/en/tools/hash_identifier

### Cracking Windows Authentication Hashes

Modern Windows OS auth hashes use NTHash/NTLM (NT in john)

### Cracking /etc/shadow Hashes

Use `unshadow /etc/passwd /etc/shadow` to generate a string for john to crack /etc/shadow 
hashes. Can also just use the user's line from /etc/passwd and from /etc/shadow

### Single Crack Mode

Gecos fields are separated by `:` and can be found in /etc/passwd and /etc/shadow

Word mangling, building a custom wordlist from modified variations of a username (joker, 
joker1, Jok3r, j0ker, J0k3r, joker!, joker*, etc.)

Use `--single` and prepend the username to the hash separated by `:` 
`<username>:<hash>`

### Custom Rules

Wiki here: https://www.openwall.com/john/doc/RULES.shtml

Define a rule: `[List.Rules:<Rule Name Here>]`

Insert string STR at position N: AN"STR"
Append a string: Az
Prepend a string: A0
Capitalise: c

### Cracking Password Protected Zip Files

Use zip2john to generate a hash format john understands then pass the hash to john as we 
would a normal hash

`zip2john <Options> <ZipFile> > <OutputFileName>`

### Cracking Password Protected RAR Archives

Use rar2john as with zip2john

### Cracking SSH Keys with John

Use ssh2john as with rar2john and zip2john

## Encryption - Crypto 101