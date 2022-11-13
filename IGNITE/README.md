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
 
 
 
