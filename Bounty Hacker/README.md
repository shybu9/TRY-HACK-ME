# Bounty Hacker 
 ~ https://tryhackme.com/room/cowboyhacker
* ### This is a direct walk-through to the flag. Hopping that you have given your best before referring to this write-up
* ### Assuming that your are using Attack-box or conntected to VPN. If not, do refer to [vpn_connection](https://github.com/shybu9/TRY-HACK-ME/tree/main/OPEN-VPN-CONNECTION "OPEN VPN CONNECTION")

<br>

![bonty intro](https://user-images.githubusercontent.com/112984045/200177378-c657e25e-8fa7-43fa-b5de-7c34aa08ee05.png)
 ~ the IP will be given once once you join the room and Start Machine succefully after connecting to OPEN-VPN
 
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
 
 * `the ftp port 21 is Anonymous FTP login allowed (FTP code 230)`
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
   
   *As we have username: 'lin' and password list: locks.txt. We can use hydra tool for bruteforcing ssh server.
   <br>command:
   ```bash
   hydra -l lin -P locks.txt ssh://<IP>
   ```
   <br>
   
   * After logging in to ssh we can look for file contents and read them using the same commands :
   ```bash 
   ls -la
   ```
   ```bash
   cat user.txt
   ```
   
   
   
   
