# Blue Team: Summary of Operations
# Table of Contents- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

![image](https://user-images.githubusercontent.com/105754955/182512289-024a9ceb-681e-4769-b7e9-0170b473d9b5.png)


The following machines were identified on the network:

- Kali
  - **Operating System**: Linux 5.4.0
  - **Purpose**: Attacking machine
  - **IP Address**: 192.168.1.90
  
- ELK
  - **Operating System**: Linux (Ubuntu)
  - **Purpose**: ELK Stack
  - **IP Address**: 192.168.1.100
  
- CAPSTONE
  - **Operating System**: Linux (Ubuntu)
  - **Purpose**: ELK Stack
  - **IP Address**: 192.168.1.100
  
- Target 1
  - **Operating System**: Linux (Ubuntu)
  - **Purpose**: Vulnerable Web server (WordPress)
  - **IP Address**: 192.168.1.110
  
 
 ### Description of Targets

The target of this attack was: `Target 1` (192.168.1.110). 

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors

 **WHEN count() GROUPED OVER top 5 'http.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes.**
 
  - **Metric**: WHEN count() GROUPED OVER top 5 http.response.status_code > 400
  - **Threshold**: Above 400
  - **Vulnerability Mitigated**: Enumeration/ Brute Force
  - **Reliability**: This alert is highly reliable. Measuring the error code that are 400 and above filter out the normal activity or successful responses. 400 and plus codes are client and server errors which are of more concern. Especially, when the error codes happen at a high rate.
 
![image](https://user-images.githubusercontent.com/105754955/182507002-8faebd9c-04b2-495f-830c-4ea68452e542.png)

#### HTTP Request Size Monitor

 **WHEN sum() of http.request.byte OVER all documents IS ABOVE 3500 FOR THE LAST 1 minute.**
 
  - **Metric**: WHEN sum() of http.request.byte OVER all documents
  - **Threshold**: Above 3500
  - **Vulnerability Mitigated**: Code injection in HTTP requests (XSS and CRLF) or DDOS
  - **Reliability**: This alert would not create an excessive amount of false positives when set the at medium reliability. There is a possiblity for a large number of non-malicious HTTP requests or legitimate HTTP traffic.

 ![image](https://user-images.githubusercontent.com/105754955/182507103-8e5be84c-9ec4-473f-b7fc-ce10a9f2f72b.png)

#### CPU Usage Monitor

**WHEN max() OF system.process.cpu.total.pct OVER all documents IS ABOVE 0.5 FOR THE LAST 5 minutes.**

  - **Metric**: WHEN max() OF system.process.cpu.total.pct OVER all documents
  - **Threshold**: Above 0.5
  - **Vulnerability Mitigated**: Malicious software, programs (malware or viruses) running taking up resources.
  - **Reliability**: This alert is highly reliable. This alert can also function to improve CPU usage.

 ![image](https://user-images.githubusercontent.com/105754955/182507136-f75fbdb8-63e5-42e9-a772-244441bd5baf.png)


### Suggestions for Going Further (Optional)
Each alert above pertains to a specific vulnerability/exploit. Recall that alerts only detect malicious behavior, but do not stop it. For each vulnerability/exploit identified by the alerts above, suggest a patch. E.g., implementing a blocklist is an effective tactic against brute-force attacks. It is not necessary to explain how to implement each patch.

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:

  **Excessive HTTP Errors**

  **Patch**: WordPress Hardening

  - Regular updates should be implemented.
  -   Installing security plugin(s)
  -   Disabling unused WordPress features and settings, e.g; WordPress XML-RPC (on by default)
  - Blocking request to /?author= by configuring web server settings.
  - Remove the WordPress logins from being publicly accessible, specifically:
          - /wp-admin
          - /wp-login.php

  **Why It Works**: 

  - Regular updates to WordPress, the PHP version and plugin is a simple way to implement patches or fixes vulnerabilities/exploits.

  Depending on the WordPress Security Plugins, features like this are added:
  
      - Malware Scans
      
      - Firewall 
   
  - REST API is used by WPScan to enumerate users. Disabiling this will help mitigate WPScan or enumeration in general.

  - XML-RPC uses HTTP as a method of data transportation.
  
  - Removal of public access to WordPress logins will help reduce the attacks.

  **HTTP Request Size Monitor**

  **Patch**: Code Injection/ DDOS Hardening

  Implementation of HTTP Requests Limit on the web server.
  
  Include input validation

  **Why It Works**: 

  Implementing a limit on an HTTP request will help reject requests that are too large by giving a 404 error.
  
  Input validation could help protect against HTTP request from the server via website or application accross.

  **CPU Usage Monitor**

  **Patch**: Virus or Malware Hardening

  Update Antivirus protection plan 
  
  Configure Host Based Intrusion Detection System (HIDS)

  **Why It Works**: 

  Antivirus protection specializes in removeral, detection and overall prevention of malicious threats against the computers.

  HIDS monitors and analyzes internals of the computing system.

