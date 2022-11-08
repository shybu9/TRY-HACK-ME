# Year of The Rabbit
    ~ https://tryhackme.com/room/yearoftherabbit

* ### This is a direct walk-through to the flag. Hopping that you have given your best before referring to this write-up
* ### Assuming that your are using Attack-box or conntected to VPN. If not, do refer to [vpn_connection](https://github.com/shybu9/TRY-HACK-ME/tree/main/OPEN-VPN-CONNECTION "OPEN VPN CONNECTION")
*  ### Do follow this page through out the line to void confusion.

<br>

 ![rabbit intro](https://user-images.githubusercontent.com/112984045/200450627-fcc3ffa8-4e3c-4bde-9530-2cdb6acfbf7b.png)
~ the IP will be given once you join the room and Start Machine succefully after connecting to OPEN-VPN
 
 ## ENUMERATION :

 * For ENUMERATION we use nmap tool which come by default in kali linux.
 <br>command : 
 ```bash
 nmap -sVC -T4 -v <IP> 
 ```
 ![rabbit nmap](https://user-images.githubusercontent.com/112984045/200450896-f5d9f661-a182-4d23-a6bd-ac86ea7ebce7.png)
 <br>~ we can add -v argument for more verbose output.
 
 * I usually perform one more scan by using my personal tool : [port-scanner](https://github.com/shy.bu9/portscanner)
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
 gobuster -u 
 ![rabbit gobuster](https://user-images.githubusercontent.com/112984045/200460412-5d6f4312-86ab-4372-805f-7875f79515a0.png)
 ![rabbit assets](https://user-images.githubusercontent.com/112984045/200452670-15edf2c8-84b6-4285-8e6b-897f3983ab95.png)

