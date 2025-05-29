# Stress-testing

# Security-Testing

## 📋 Table of Contents

1. [Security Testing Objectives](#%EF%B8%8F-a-brief-description-of-the-acs-security-testing-project)
2. [Hardware and software](#-the-hardware-and-software-used)
   - [Hardware](#1-%EF%B8%8F-hardware)
   - [Software](#2--software-acs)
   - [Testing Software](#3-%EF%B8%8F-testing-software-kali-linux)
3. [Kali Linux Tools](#-additional-kali-linux-tools)
   - [Vulnerability scanners](#%EF%B8%8F-1-vulnerability-scanners)
   - [Traffic Analysis Tools](#-2-traffic-analysis-tools)
   - [Attacks on web applications](#-3-attacks-on-web-applications)
   - [Password analysis tools](#%EF%B8%8F-4-password-analysis-tools)
   - [Social engineering and phishing](#-5-social-engineering-and-phishing)
   - [Checking wireless networks](#-6-checking-wireless-networks)
   - [Frameworks and automation](#%EF%B8%8F-7-frameworks-and-automation)
4. [Denial of Service Testing (DoS/DDoS)](#%EF%B8%8F-denial-of-service-testing-dosddos)
   - [Attacks used in testing](#-attacks-used-in-testing)
   - [Using the hping3 tool](#-using-the-hping3-tool)
   - [Test results](#test-results)
5. [HTTP/2 Rapid Reset attack](#-http2-rapid-reset-attack)
   - [The principle of operation](#-the-principle-of-operation)
   - [Implementation of the attack](#%EF%B8%8F-implementation-of-the-attack)
   - [Results and protection](#%EF%B8%8F-results-and-protection)
6. [Brute Force Password Brute Force](#-brute-force-password-brute-force)
   - [Attack using Fuzz and dictionary rockyou.txt](#%EF%B8%8F-attack-using-fuzz-and-dictionary-rockyoutxt)
   - [Example of the Wfuzz commands](#-example-of-the-wfuzz-commands)
   - [System response](#-system-response-code-403)
   - [Protection mechanisms](#%EF%B8%8F-protection-mechanisms-ip-blocking)
7. [General conclusions about the security of ACS](#-general-conclusions-about-the-security-of-acs)
8. [Recommendations for improving security](#-recommendations-for-improving-security)
9. [Opportunities for future research](#-opportunities-for-future-research)
10. [Authors](#authors)

## 🛡️ A brief description of the ACS security testing project

The information security level of the access control and management system (ACS) with two-factor authentication has been tested.
A specialized Kali Linux distribution was used for testing, which includes security audit and penetration testing tools.
Testing objectives:

1 Identification of vulnerabilities in the components of the ACS: the database web interface, the dashboard, and the backend;

2 Assessment of resistance to typical cyber attacks.

Conducted tests:

1 DoS/DDoS attacks (Ping flood, UDP flood, SYN flood) using hping3 utility;

2 An HTTP/2 Rapid Reset attack aimed at overloading the server through the thread reset mechanism;

3 Brute Force Attack on Login Forms Using Fuzz and Password Dictionary rockyou.txt .

## 🧰 The hardware and software used

The following hardware and software tools were used as part of the security testing project for the access control and management system (ACS):

### 1. 🖥️ Hardware:

- Arduino Uno is a microcontroller used to read RFID tags;

- RFID module RC522 is a radio frequency identification module for reading MIFARE cards;

- USB camera — used for biometric authentication by facial recognition;

- Server/PC running Windows 11 — to run the web interface, dashboard, and analysis;

- A local network is used to organize the interaction of ACS components and conduct network attacks.

### 2. 🧑‍💻 Software (ACS):

- Flask (Python) is a web framework for creating a REST API, dashboard, and administration interface.;

- MySQL — a database for storing information about users and access events;

- Flask-Limiter — a module for limiting the frequency of requests (protection against DoS and brute force);

- OpenCV and face_recognition are libraries for face recognition at the entrance;

- FlaskWebGUI is a wrapper for running the interface as a desktop application;

- A custom Python script implements IP address filtering, attacker blocking, and event logging.

### 3. 🖥️ Testing Software (Kali Linux):

- Kali Linux OS is a specialized distribution for conducting security audits and penetration testing;

- hping3 is a network packet generation utility for implementing DoS attacks (Ping Flood, SYN Flood, UDP Flood);

- Wfuzz is a tool for brute force attacks on web forms;

- iptables is a configurable firewall in Linux (as an alternative to Windows Firewall).

## 🔧 Additional Kali Linux Tools

Kali Linux includes more than 600 utilities covering all stages of penetration testing and security analysis. In addition to the tools already used in the project (hping3, Wfuzz, Nmap, etc.), the distribution package includes the following categories and utilities:

### 🛠️ 1. Vulnerability scanners

1 OpenVAS is a powerful system for comprehensive vulnerability analysis;

2 Vulners and Searchsploit — databases of exploits and vulnerabilities;

3 Skipfish is a web application security scanner with support for automatic report generation.

### 🌐 2. Traffic Analysis Tools

1 Wireshark is a powerful network traffic sniffer and analyzer;

2 tcpdump is a command—line tool for capturing network packets;

3 Ettercap is a utility for Man-in-the-Middle (MITM) attacks.

### 📂 3. Attacks on web applications

1 OWASP ZAP (Zed Attack Proxy) is an alternative to Burp Suite, used to find vulnerabilities in web applications;

2 sqlmap is an automatic tool for performing SQL injections and capturing data from a database.;

3 Commix is a tool for testing for the presence of command injections.

### 🛡️ 4. Password analysis tools

1 Hydra is a utility for sorting passwords using various protocols (FTP, SSH, HTTP, etc.);

2 Medusa is an analog of Hydra with the possibility of mass search;

3 Crunch — dictionary generator for brute force;

4 CeWL is a dictionary generator based on web content (for example, a company website).

### 🧠 5. Social engineering and phishing

1 Social Engineering Toolkit (SET) is a powerful framework for simulating phishing attacks, creating fake websites, etc.;

2 BeEF (Browser Exploitation Framework) is a tool for attacking vulnerable browsers and extensions.

### 🔒 6. Checking wireless networks

1 Aircrack-ng — a set of tools for hacking WEP/WPA/WPA2 networks;

2 Reaver — a tool for bruteforcing WPS PIN codes;

3 Kismet is a system for detecting wireless networks and monitoring traffic.

### 🏗️ 7. Frameworks and automation

1 The Metasploit Framework is one of the most popular frameworks for vulnerability development and exploitation.;

2 ExploitDB + msfconsole is an exploit database with the ability to run automatically through the Metasploit console;

3 Armitage is a GUI shell for Metasploit that simplifies attacks.

These tools make Kali Linux a universal platform for testing information security systems, including ACS, web applications, databases, local and wireless networks.

## 🛡️ Denial of Service Testing (DoS/DDoS)

Denial of service testing using various types of DoS and DDoS attacks was conducted to assess the stability of the access control and management system (ACS), as well as related components (dashboards and the database web interface).

### 📶 Attacks used in testing

#### Ping Flood 

Is an attack in which an attacker sends a large number of ICMP Echo requests (ping) to the target host, depleting its resources for processing responses.

#### UDP Flood 

Numerous UDP packets are generated on random ports of the target machine. In response, the system is forced to send ICMP packets "port unavailable", which puts strain on the network and resources.

#### SYN Flood 

Is an attack at the stage of establishing a TCP connection. The attacker initiates many connections without completing them, as a result of which the target server spends resources "waiting" for the connections to complete.

### 🔧 Using the hping3 tool

The hping3 tool included in Kali Linux was used to simulate the above attacks. It allows you to flexibly generate arbitrary packets and control various network communication parameters (protocol type, TCP flags, frequency of sending, etc.).

The commands to launch the attacks looked like this:

#### Ping Flood:

````
hping3 -1 -d 120 -s 12345 -p 80 --flood <target IP address>
hping3 -1 -c 1000 --faster <target IP address> -V
hping3 -1 --flood <target IP address>
hping3 -1 --flood -a 1.2.3.4 <target IP address>
````

-1 - using the ICMP protocol (ping).

--flood - sending packages as quickly as possible, without waiting for responses

-d 120 - the size of the data in the packet (120 bytes).

--fast - fast mode (but not maximum, like --flood).

--count 100 - send 100 packages.

-a 1.2.3.4 is the fake (spoofed) IP address of the sender.

![image](https://github.com/user-attachments/assets/9131180b-f4c7-4652-8fa2-2e257653bcbd)

#### UDP Flood:

````
hping3 --udp -p 80 --flood <target IP address>
hping3 -2 -c 1000 --faster -p 1000 <target IP address> -V
hping3 --udp --flood -a 1.2.3.4 <<target IP address>
````

--udp - using the UDP protocol.

-- 2 - using the UDP protocol.

--flood - sending packages as fast as possible.

-p 1000 - indicates the destination port.

-d 120 is the size of the data in the UDP packet.

-a 1.2.3.4 - spoofing of the sender's IP address.

![image](https://github.com/user-attachments/assets/27f1fc4f-589f-4200-9c97-93ca4b85ce35)

#### SYN Flood:

````
hping3 --syn -p 80 --flood <target IP address>
hping3 --syn -c 1000 --faster -p 3000 <target IP address>
hping3 --syn -p 80 -a 1.2.3.4 --flood <target IP address>
````

--syn - sending TCP packets with the SYN flag.

--flood - the maximum sending speed.

-p 3000 is the destination port.

-c 10000 is the number of packets to send.

-a 1.2.3.4 - substitution of the source IP address.

![image](https://github.com/user-attachments/assets/33e9c75c-b6bb-49e1-80b4-04ca185e1601)

### Test results

In all cases, when launching attacks, it was not possible to disrupt the performance of the ACS and its components. All incoming malicious packets were successfully rejected during the network interaction stage. This is confirmed by the absence of failures in the web interface and dashboard, as well as logging logs.

The main reason for the failure of the attacks was the pre-configured security policy of the built-in Windows firewall, which blocks all incoming traffic that does not comply with the allowed rules. Similar protection can be implemented in Linux systems using iptables.
Conclusions on sustainability

The DoS/DDoS attacks carried out have shown the high resistance of ACS to external network influences. Pre-configuring the firewall allows you to effectively filter malicious traffic.

Additionally, if the system is deployed in a production environment, it is recommended to use dedicated network equipment, such as pfSense, for more flexible and centralized traffic filtering.

## 🚨 HTTP/2 Rapid Reset attack

### 🔍 The principle of operation

The HTTP/2 Rapid Reset attack uses the specifics of the HTTP/2 protocol, in which a client can establish multiple parallel streams within a single connection. The attacker opens a large number of such streams and immediately dumps them using the RST_STREAM frame. This leads to an excessive load on the server, as it is forced to allocate resources to process each of these requests, despite their instant reset.

As a result, the server spends significant computing and network resources on managing a multitude of incomplete threads, which reduces its performance and can lead to denial of service (DoS) attacks.

### ⚙️ Implementation of the attack

Specialized tools or custom scripts were used to carry out the attack, which:

1 Establish multiple parallel HTTP/2 connections to the server.

2 Streams are quickly initiated on each connection.

3 RST_STREAM frames are immediately sent to reset each stream.

4 Repeat this process with high frequency, creating a constant load.

In our testing, these requests were sent from a single IP address, which allowed us to identify the possibility of blocking an attacker based on traffic analysis.

### 🛡️ Results and protection

During the attack, the server began to experience increased load, which was recorded by monitoring. As a result of the automatic analysis of the frequency of requests, an abnormally high level of activity was detected from a specific IP address.

To protect against this attack, a Python script was implemented that:

- Monitors the number of HTTP/2 requests from each IP address.

- If more than 5 requests are received from one IP address in one second, the IP address is automatically added to the block list.

- The blocked IP cannot access the system for one hour.

- After the blocking time expires, the IP is removed from the list and gets the opportunity to connect again.

````
limiter = Limiter( 
    get_remote_address,
    app=app,
    default_limits=["5 per second"]
)

blocked_users = {} 

def check_blocked_user(ip):
    if ip in blocked_users:
        block_time = blocked_users[ip]
        if time.time() - block_time > 3600:
            del blocked_users[ip]
            return False
        return True
    return False

def block_response():
    return Response(
        "<h1 style='color: red; font-size: 48px; text-align: center;'>Вы заблокированы</h1>",
        status=403,
        content_type='text/html; charset=utf-8'
    )

@app.before_request
def before_request():
    ip = get_remote_address() or request.remote_addr
    if check_blocked_user(ip):
        return block_response()

@app.errorhandler(429)
def ratelimit_error(e):
    ip = get_remote_address() or request.remote_addr
    if ip:
        blocked_users[ip] = time.time()
    return block_response()
````
This mechanism allowed:

![image](https://github.com/user-attachments/assets/eed4d1b6-766a-4d12-bec4-4100abb123b2)

![image](https://github.com/user-attachments/assets/7ab73194-4d6e-43f3-a530-9574455e1462)

1 Block the attacker's IP address, excluding him from access to the system.

2 Save access to the system for legitimate users from other IP addresses.

3 To prevent further use of the HTTP/2 Rapid Reset attack for the duration of the block.

This method provided a balance between protection and ease of use of the system, minimizing false alarms and keeping the ACS operational for honest users.

## 🔐 Brute Force Password Brute Force

### 🛠️ Attack using Fuzz and dictionary rockyou.txt

To assess the vulnerability of ACS to password brute force attacks, the Wfuzz tool was used, a specialized fuzzer for the HTTP protocol that allows you to automatically insert password options from the dictionary into the authorization form.

A popular file was used as a dictionary. rockyou.txt — a collection of real passwords obtained from hacked databases. To speed up testing, the dictionary has been reduced to 2,000 of the most common passwords.

### 🎯 Example of the Wfuzz commands

Example 1: Brute-forcing a password through a POST request with the password parameter

````
wfuzz -c -z file,rockyou_short.txt -d "username=admin&password=FUZZ" -H "Content-Type: application/x-www-form-urlencoded" http://target-url/login
````

Substitutes words from the file rockyou_short.txt in the password field.

Example 2: Going through the password and login at the same time (double substitution)

````
wfuzz -c -z file,usernames.txt -z file,rockyou_short.txt -d "username=FUZZ1&password=FUZZ2" -H "Content-Type: application/x-www-form-urlencoded" http://target-url/login
````

Uses two dictionaries: one for logins (usernames.txt ), another for passwords (rockyou_short.txt ).

Example 3: Password brute force in a GET request (for example, authentication via URL parameters)

````
wfuzz -c -z file,rockyou_short.txt http://target-url/login?username=admin&password=FUZZ
````

Example 4: Password brute force using session cookies

````
wfuzz -c -z file,rockyou_short.txt -d "username=admin&password=FUZZ" -H "Content-Type: application/x-www-form-urlencoded" -b "sessionid=abcdef123456" http://target-url/login
````

The -b option adds a cookie to the request.

Example 5: Brute-forcing a password with the User-Agent header

````
wfuzz -c -z file,rockyou_short.txt -d "username=admin&password=FUZZ" -H "Content-Type: application/x-www-form-urlencoded" -H "User-Agent: Mozilla/5.0" http://target-url/login
````

Example 6: Displaying only successful attempts (for example, HTTP codes are not 403)

````
wfuzz -c -z file,rockyou_short.txt -d "username=admin&password=FUZZ" -H "Content-Type: application/x-www-form-urlencoded" --hc 403 http://target-url/login
````

The --hc 403 (hide codes) parameter hides all responses with the 403 code so as not to clog the output.


### 🚫 System response (code 403)

![image](https://github.com/user-attachments/assets/c2b62c91-7dc8-4602-96bd-90ed1f07979f)

![image](https://github.com/user-attachments/assets/f59d2a15-b7a8-4fd5-8c35-69be104be832)

![image](https://github.com/user-attachments/assets/2c10245f-0ee1-4b91-97e1-5843c549df26)

In all cases, the server returned the HTTP status code 403 Forbidden when the allowed number of authorization attempts was exceeded.

This indicates the successful activation of protective mechanisms that:

- They track the number of failed attempts from each IP.

- They restrict further access by blocking the attacker's IP.

### 🛡️ Protection mechanisms (IP blocking)

The protection is implemented using a Python script integrated into the server application. The main functions of the code:

- Maintaining a counter of requests from each IP.

- If the limit is exceeded (for example, more than 5 failed attempts per second), the IP is added to the block list.

- A blocked IP cannot send requests within the set time (usually 1 hour).

- After the time expires, the lock is lifted.

An example of a code snippet implementing IP blocking:

````
ALLOWED_IPS = {'127.0.0.1', '192.168.100.2', '192.168.100.147'}

@app.before_request 
def limit_remote_addr():
    if request.remote_addr not in ALLOWED_IPS:
        abort(403)
````

## ✅ General conclusions about the security of ACS

During the comprehensive testing of the access control and management system (ACS), its main components were tested for resistance to common types of attacks, including DoS/DDoS, HTTP/2 Rapid Reset, and Brute Force attacks.

The results showed that the system successfully blocks network overload attempts (DoS/DDoS), thanks to properly configured firewalls and built-in firewall rules. The mechanism for blocking IP addresses when suspicious activity is detected, for example, during an HTTP/2 Rapid Reset attack and frequent unsuccessful authorization attempts, has demonstrated high efficiency, preventing further attacks by intruders without impairing access for legitimate users.

In addition, the web interface and dashboard are equipped with password brute force protection, which is confirmed by server responses with the 403 code during brute force attempts. The IP address filtering mechanism used limits attacks from unknown sources.

The use of modern tools and technologies such as Kali Linux, hping3 and Wfuzz has made it possible to identify potential vulnerabilities and eliminate them in a timely manner, which increases the overall security level of the system.

## 🔧 Recommendations for improving security

To further enhance the security of the ACS, it is recommended to regularly update the software and signature databases for protection tools to take into account new threats and exploits.

It is recommended to implement two-factor authentication (2FA) for access to administrative dashboards and critical system components. This will make unauthorized access much more difficult, even if the password is compromised.

It is also important to implement centralized monitoring and notification of suspicious activity using SIEM systems (Security Information and Event Management), which will allow for rapid response to threats.

To increase traffic control, you can deploy an additional firewall (for example, pfSense) with more flexible filtering rules.

In addition to technical measures, it is recommended to conduct periodic training events and trainings for employees in order to minimize the risks associated with the human factor.

Finally, it is worth regularly conducting automated and manual security testing (penetration testing) using the extended Kali Linux toolkit.

## 🔍 Opportunities for future research

In the field of future research, it is promising to consider the development and implementation of adaptive anomaly detection systems (IDS/IPS) capable of automatically responding to new types of attacks.

It is also important to explore the possibilities of using machine learning to analyze network traffic and user behavior in order to prevent complex attacks.

Research can be aimed at identifying vulnerabilities of new protocols and technologies used in ACS and developing methods to protect them.

The integration of mobile solutions and biometric systems is promising to increase the convenience and security of access.

In addition, it is worth paying attention to the research and implementation of cryptographic methods for protecting communication channels and user data.

# Authors
If you have any questions, you can ask them to us by writing to us at email:
- ivanovvvvvvvanton3829@gmail.com
