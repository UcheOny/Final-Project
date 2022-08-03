**Network Analysis**

**Time Thieves**

At least two users on the network have been wasting time on YouTube. Usually, IT wouldn't pay much mind to this behavior, but it seems these people have created their own web server on the corporate network. So far, Security knows the following about these time thieves:

•	They have set up an Active Directory network.

•	They are constantly watching videos on YouTube.

•	Their IP addresses are somewhere in the range **10.6.12.0/24**

    •	**Filter: ip.addr==10.6.12.0/24**
    
**You must inspect your traffic capture to answer the following questions:**

1.	What is the domain name of the users' custom site?

    **Answer: Frank-n-Ted-DC.frand-n-ted.com**

2.	What is the IP address of the Domain Controller (DC) of the AD network?

    **Answer: 10.6.12.12**
	
 ![image](https://user-images.githubusercontent.com/105754955/182510230-f375c2a0-b4a4-4442-b9c3-7e71aa8e7276.png)


3.	What is the name of the malware downloaded to the 10.6.12.203 machine? Once you have found the file, export it to your Kali machine's desktop.

    **Answer: June11.dll**

    **Filter: ip.addr==10.6.12.203 and http.request.method==GET**
    
 ![image](https://user-images.githubusercontent.com/105754955/182510263-bba99617-27e0-40c0-babf-623c814c1e2c.png)

4.	Upload the file to VirusTotal.com. 

    What kind of malware is this classified as? 

    **Answer: Trojan**
  
  ![image](https://user-images.githubusercontent.com/105754955/182510293-80f70f81-2894-44d3-9525-df5f096ea2b5.png)

**Vulnerable Windows Machines**

The Security team received reports of an infected Windows host on the network. They know the following:

•	Machines in the network live in the range 172.16.4.0/24.

•	The domain mind-hammer.net is associated with the infected computer.

•	The DC for this network lives at 172.16.4.4 and is named Mind-Hammer-DC.

•	The network has standard gateway and broadcast addresses.

  **•	Filter: ip.src == 172.16.4.4**

**Inspect your traffic to answer the following questions:**

1.	Find the following information about the infected Windows machine:

        o	Host name: **ROTTERDAM-PC**
        
        o	IP address: **172.16.4.205**

        o	MAC address: **00:59:07:b0:63:a4**
 
![image](https://user-images.githubusercontent.com/105754955/182510475-bcd35893-5836-4eb2-a254-4b34d65baa6d.png)

2.	What is the username of the Windows user whose computer is infected?

    **Answer: Matthijs.devries**
    
    **Filter; ip.src==172.16.4.4 and kerberos.CNameString**
    
  ![image](https://user-images.githubusercontent.com/105754955/182510637-f008f721-85a4-49d2-9e85-a0ec84dd9a16.png)

3.	What are the IP addresses used in the actual infection traffic?
 	
    **Answer: 166.62.111.64 and 185.243.115.84**
 
 ![image](https://user-images.githubusercontent.com/105754955/182510674-80259098-3da7-483c-8bf3-498f64471050.png)

4.	As a bonus, retrieve the desktop background of the Windows host.

 ![image](https://user-images.githubusercontent.com/105754955/182510710-44a51011-cb5f-42a5-9c69-49e2074eae2e.png)

**Illegal Downloads**

IT was informed that some users are torrenting on the network. The Security team does not forbid the use of torrents for legitimate purposes, such as downloading operating systems. However, they have a strict policy against copyright infringement.

IT shared the following about the torrent activity:
    
    •	The machines using torrents live in the range 10.0.0.0/24 and are clients of an AD domain.
    
    •	The DC of this domain lives at 10.0.0.2 and is named DogOfTheYear-DC.
    
    •	The DC is associated with the domain dogoftheyear.net.

**Your task is to isolate torrent traffic and answer the following questions in your Network Report:**

1.	Find the following information about the machine with IP address 10.0.0.201:
        
        **o	MAC address: 00:16:17:18:66:c8**
        
        **o	Windows username: elmer.blanco**
        
        **o	OS version: BLANCO-DESKTOP**
        
        **o	Filter: ip.src == 10.0.0.201 and kerberos.CNameString**
 

![image](https://user-images.githubusercontent.com/105754955/182510940-9413e39c-7e58-46b2-b06e-72c093c12f6f.png)

2.	Which torrent file did the user download?

    **Answer: **Betty_Boop_Rythm_on_the_Reservation.avi.torrent.**
    
    **Filter: ip.addr== ip.addr==10.0.0.201 and (http.request.uri contains ".torrent")**

![image](https://user-images.githubusercontent.com/105754955/182511444-f602daac-0e53-4094-b8f9-bb4f316a239f.png)
