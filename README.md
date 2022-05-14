# Blue Team: Summary of Operations
### Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

## Network Topology & Network Diagram

The following machines were identified on the network during the initial scan.

### Kali

-  Operating System: Debian Kali 5.4.0
-  Purpose: The attacking machine used to conduct the pen test
-  IP Address: 192.168.1.90

### ELK

-  Operating System: Ubuntu 18.04
-  Purpose: ELK monitoring stack
-  IP Address: 192.168.1.100

### Target 1

-  Operating System: Debian GNU/Linux 8
-  Purpose: Vulnerable Wordpress host, sends logs to ELK
-  IP Address: 192.168.1.110

### Capstone

-  Operating System: Ubuntu 18.04
-  Purpose: Vulnerable Web Server
-  IP Address: 192.168.1.105

**Network Diagram**

<img src="https://github.com/Tamie13/Blue-Team-Summary-of-Operations/blob/main/Final%20Project%20Image%20Folder/Network%20Topology.png" width="650" height="600">


### Description of Targets

-  The target of this attack was Target 1 (192.168.1.110)
-  Target 1 is an Apache web server and is SSH enabled, so ports 80 and 22 are potential points of entry, which was verified by the nmap scan below

#### Monitoring the Targets
**Target 1**

-  Port 22/TCP Open SSH OpenSSH 6.7p1 Debian 5+deb8u4
-  Port 80/TCP Open HTTP Apache httpd 2.4.10 (Debian)


## ALERTS & SUGGESTED PATCHES FOR VULNERABILITIES

-  Each alert below pertains to a specific vulnerability/exploit.
-  The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the information generated from the alerts below. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:

-  Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### ALERT 1 - Excessive HTTP Errors

    -  WHEN count() GROUPED OVER top 5 'http.response.status_code' IS ABOVE 400 FOR THE LAST 5 minutes

       -  Metric:  WHEN count() GROUPED OVER top 5 ‘http.response.status_code’
       -  Threshold:  IS ABOVE 400
       -  Vulnerability Mitigated:  Enumeration/Brute Force
       -  Reliability:  The alert has demonstrated highly reliability
<img src="https://github.com/Tamie13/Blue-Team-Summary-of-Operations/blob/main/Final%20Project%20Image%20Folder/Excessive%20HTTP%20Errors%20Alert.png" width="650" height="600">

-  **Vulnerability 1** *BRUTE FORCE*
    -  Patch: WordPress Hardening Maintenance Plan
       -  Require regular checks and installation of updates for WordPress Core, PHP and Plugins
       -  Check for and disable unused features in Wordpress
       -  Admin and login/s should not be publicly accessible
    -  Why It Works: The number of attacks by hackers and bad actors on WordPress increases everyday.
Regular maintenance to keep your WordPress site hardened is the best way to proctect against those
attacks as well as fix bugs on the site.  It ensures your WP site is working as smoothly as possible
and is "less likely" to be the victim of malicious attacks.

#### ALERT 2 - HTTP Request Size Monitor

    -  WHEN sum() of http.request.bytes OVER all documents IS ABOVE 3500 FOR THE LAST 1 minute

       -  Metric:  WHEN sum() of http.request.bytes OVER all documents
       -  Threshold:  IS ABOVE 3500
       -  Vulnerability Mitigated:  Code injection in HTTP requests (XSS and CRLF) or DDOS
       -  Reliability:  This alert demonstrates medium reliability, as it has produced some false positives

<img src="https://github.com/Tamie13/Blue-Team-Summary-of-Operations/blob/main/Final%20Project%20Image%20Folder/HTTP%20Reques%20Size%20Monitor%20Alert.png" width="650" height="600">

-  **Vulnerability 2** *Code injection in HTTP requests (XXS and CRLF) or DDOS
    -  Patch: Harden Agaisnt DDOS / Code Injection Attacks
       -  Disable XML RPC to prevent third party apps access to your website
       -  Disable REST API to prevent unneccessary access to your data
       -  Activate a website application firewall
    -  Why It Works:

(Site referenced for DDOS hardening suggestions: https://www.wpbeginner.com/wp-tutorials/how-to-stop-and-prevent-a-ddos-attack-on-wordpress/ )

#### ALERT 3 - CPU Usage Monitor

    -  WHEN max() OF system.process.cpu.total.pct OVER all documents IS ABOVE 0.5 FOR THE LAST 5 minutes

       - Metric:  WHEN max() OF system.process.cpu.total.pct OVER all documents
       - Threshold:  IS ABOVE 0.5
       - Vulnerability Mitigated:  Malicious software, programs (malware or viruses) running taking up resources
       - Reliability:  The alert demonstrates high reliability

<img src="https://github.com/Tamie13/Blue-Team-Summary-of-Operations/blob/main/Final%20Project%20Image%20Folder/CPU%20Usage%20Monitor%20Alert.png" width="650" height="600">

-  **Vulnerability 3** *Malicious software or program running taking up resources
    -  Patch:
    -  Why It Works:

