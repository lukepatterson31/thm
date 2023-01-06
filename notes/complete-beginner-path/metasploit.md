# Metasploit: Introduction

### Main Components of Metasploit

`msfconsole`

- **Exploit:** A piece of code that uses a vulnerability present on the target system.

- **Vulnerability**: A design, coding, or logic flaw affecting the target system. The 
exploitation of a vulnerability can result in disclosing confidential information or allowing 
the attacker to execute code on the target system.

- **Payload**: An exploit will take advantage of a vulnerability. However, if we want the 
exploit to have the result we want (gaining access to the target system, read confidential 
information, etc.), we need to use a payload. Payloads are the code that will run on 
the target system.

### Modules

`/usr/share/metasploit-framework/modules`

**Auxiliary**: Any supporting module (scanners, crawlers and fuzzers) can be found here

**Encoders**: Encoders will allow you to encode the exploit and payload in the hope that a 
signature-based antivirus solution may miss them.

**Evasion**: While encoders will encode the payload, they should not be considered a direct 
attempt to evade antivirus software. On the other hand, “evasion” modules will try that, with 
more or less success.

**Exploits**: Exploits, neatly organised by target system

**NOPs**: NOPs (No OPeration) do nothing, literally.In the Intel x86 CPU family they are 
represented with 0x90, following which the CPU will do nothing for one cycle. They are often 
used as a buffer to achieve consistent payload sizes.

**Payloads**: Payloads are code that will run on the target system.

- **Singles**: Self-contained payloads (add user, launch notepad.exe, etc.) that do not need 
to download an additional component to run.

- **Stagers**: Responsible for setting up a connection channel between Metasploit and the 
target system. Useful when working with staged payloads. “Staged payloads” will first upload 
a stager on the target system then download the rest of the payload (stage). This provides 
some advantages as the initial size of the payload will be relatively small compared to the 
full payload sent at once.

- **Stages**: Downloaded by the stager. This will allow you to use larger sized payloads.

**Post**: Post modules will be useful on the final stage of the penetration testing process 
listed above, post-exploitation.

### Msfconsole

`search` can use various tags `cve:`, `type:`
`use <module name OR search result #>` to select module
`show options` to display exploit options
`show payloads` to display exploit payloads
`back` to leave the module
`info` for more information about the module
`set <parameter name> <value>` set a parameter
`setg <parameter name> <value>` set a global parameter
`unset <parameter name>` clear parameter
`unsetg <parameter name>` clear global parameter
`unset all` clear all parameters
`exploit` or `run` to execute the module, add `-z` tag to background session
`check` supported by some modules, checks target system is vulnerable without exploiting it
`background` background the session prompt
`sessions` view existing sessions
`sessions -i <session number>` interact with a session

Ranking: https://github.com/rapid7/metasploit-framework/wiki/Exploit-Ranking

# Metasploit: Exploitation

### Scanning

#### Port Scanning

nmap directly
`search portscan`

#### UDP service Identification

`scanner/discovery/udp_sweep` to identify UDP services

#### SMB Scans

`search type:auxiliary scanner smb` for all SMB scanners

*Don't forget to scan for the more exotic services like NetBIOS*

### The Metasploit Database

