# Stress-testing

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

## 📌 Purpose of testing

The purpose of load testing is to evaluate the performance, resilience, and fault tolerance of key components of an access control and management system (ACS) while simultaneously processing a large number of requests. Testing allows us to determine how effectively the system copes with the peak load that occurs in the case of simultaneous actions by multiple users, such as entry/exit via RFID cards, biometric authentication, registration of new users and monitoring of events in real time.

Assigned tasks:

- Estimate the average response time for each ACS component under load;

- Check the system's ability to process a large number of parallel requests without errors;

- Determine the throughput of the system (Throughput);

- Make sure that the indicators are within acceptable limits, allowing the use of ACS in real time.;

- Identify potential bottlenecks or vulnerable architecture elements that may cause performance degradation.

The test results will be used for:

- Confirmation of the suitability of the system for operation under high load conditions;

- Optimizing component configuration (if necessary);

- Preparing to scale the system as the number of users increases.

## 🧪 Testing objects

As part of the load testing, the key components of the access control and management system (ACS) were identified, which are subject to testing for resistance to high load. Each component performs a critical function and must demonstrate stable performance under heavy use.

### 📶 RFID Authentication module

This module processes input/output requests based on user identification using RFID cards. The testing is aimed at verifying the system's ability to simultaneously handle multiple requests from different RFID devices.

### 🧍‍♂ Biometric authentication module

Responsible for face recognition and making decisions about granting access. The purpose of the test is to evaluate the behavior of the system with simultaneous biometric verification of several users.

### 📝 Web interface for registering new users

This component provides interaction with the database when new users are added. The stability of the interface is checked during mass registration.

### 📊 Dashboard

A component that displays access events and the current status of users in real time. Its ability to handle frequent updates and display up-to-date information without delay is being tested.

## 🛠 Testing Tool

To perform load testing of the access control and management system (ACS), the Apache JMeter tool was used, a powerful and extensible open source software designed to test the performance and behavior of web applications and various servers under load.

### 💡 The rationale for choosing Apache JMeter

Apache JMeter was chosen for the following reasons:

- Support for various protocols: including HTTP(S), JDBC, LDAP, FTP, TCP and others, which allows flexible modeling of different types of load.

- Flexible configuration of load scenarios: the ability to set the number of virtual users, the frequency of requests, pauses between actions and a gradual increase in load.

- Visualization of results: Provides graphs, tables, and reports that allow you to evaluate system performance using a variety of metrics.

- Plugin and script support: allows you to extend standard functions using third-party extensions and writing logic in BeanShell, Groovy or JSR223.

- Automation and repeatability of tests: the ability to re-run identical scenarios to analyze the stability of the results.

### 🧩 JMeter Configuration

An individual configuration has been created in JMeter for each test scenario. Basic settings:

1 Number of virtual users (Threads): 200

2 Acceleration time of the load (Ramp-Up Period): 5 seconds — users connect gradually during this time.

3 Number of requests per user: 10 — each user executes 10 HTTP requests sequentially.

4 Total number of requests: 2000 (200 users × 10 requests)

5 Request Type: HTTP Request

6 Method: POST (or GET, depending on the component under test)

7 Request body: contained parameters that mimic real user actions (for example, RFID code, photo, registration data, etc.)

8  Content-Type: application/json or application/x-www-form-urlencoded, depending on the API.

### 🌐 The elements of the Test Plan used

Each load test included the following elements:

1 The Thread Group - Determines the number of users, duration, and intensity of the test.

2 HTTP Request Sampler - Specific HTTP requests sent to the server. The URL, method, parameters, and headers were configured.

3 CSV Data Set Config (if necessary) - It was used to provide unique data (for example, different RFID codes or users) from a file.

4 JSON Extractor / Regular Expression Extractor - The values were extracted from the server responses for analysis and subsequent use in other requests.

5 Asserts (Checks) - were used to validate the correctness of the response (for example, the presence of the 200 OK code or a specific text in the response).

6 View Results Tree / Summary Report / Aggregate Report / Graph Results - Various visual tools for analyzing test results.

## 🛠 Description of load scenarios

To assess the stability of the access control and management system (ACS) under high load, individual load testing scenarios were developed for each key component. Each of the scenarios simulates the behavior of a large number of users accessing the system at the same time. This allowed us to obtain real performance indicators and identify potential bottlenecks.

### 📶 Scenario for the RFID authentication module

Purpose: To test the system's ability to process simultaneous access requests using RFID cards.

- Number of users: 200 virtual threads (users)

- Request type: POST to the endpoint processing RFID cards (for example, /rfid/auth)

- Request body: A JSON object of the form { "rfid": "04A3B2..." }

- Number of iterations per user: 10

- Pause between requests: minimal to simulate a massive data flow

- Expected result: HTTP 200 OK, response with full name and access status

### 🧍Script for the biometric authentication module

The goal: To evaluate the performance of the system while simultaneously recognizing faces from cameras of different users.

- Number of users: 200

- Request type: POST to an endpoint that performs biometric authentication (for example, /face/auth)

- Request format: multipart/form-data with an embedded face image

- Image upload simulation: using fictitious photos of one or more users

- Number of iterations: 10 per thread

- Expected result: HTTP 200 OK, JSON with the field { "recognized": true/false }

### 📝 Script for the page for adding a new user to the database

The goal: To check the stability of the registration web interface when adding new users en masse.

- Number of users: 200

- Request type: POST to the endpoint that processes registration (for example, /register)

- Body format: application/json with parameters:

````
    {
"full_name": "Ivanov Ivan Ivanovich",
"rfid": "04A3B2...",
"photo": "base64 representation"
    }
````

- Simulation of unique data: via CSV Data Set Config

- Number of iterations: 10

- Expected result: successful addition to the database (HTTP 201 or 200)

### 📊 Dashboard script

Purpose: To test the stability of the monitoring page under heavy load, simulating the constant updating of data on inputs and outputs.

- Number of users: 200

- Request type: GET to the endpoint of the dashboard (for example, /monitor)

- Refresh rate: every 1-2 seconds (simulating real interface auto-refresh)

- Duration of the test: 1 minute

- Expected result: correct JSON or HTML with up-to-date information on each input/output

## 📈 Load testing results

During load testing, quantitative performance indicators of the ACS components were obtained, reflecting their behavior when processing a large volume of simultaneous requests. Testing was carried out using the Apache JMeter tool, with each session simulating 200 virtual users sending 10 requests within 5 seconds. The total load was 2000 requests for each module under test.

### 🪪 Results for the RFID authentication module

| Request Label | Total Requests | Avg. Response Time (ms) | 90% Completed Within (ms) | Errors | Throughput (req/s) |
|---------------|----------------|--------------------------|-----------------------------|--------|---------------------|
| HTTP Request  | 2000           | 177                      | 288                         | 0      | 294.9               |

![image](https://github.com/user-attachments/assets/30abfb4e-a1c8-45d8-88b8-62cd70802927)

Conclusion: The RFID module has shown stable operation, error-free and high throughput, which indicates that it is ready for use in conditions of intense input flow.

### 🧠 Results for the biometric authentication module

| Request Label | Total Requests | Avg. Response Time (ms) | 90% Completed Within (ms) | Errors | Throughput (req/s) |
|---------------|----------------|--------------------------|-----------------------------|--------|---------------------|
| HTTP Request  | 2000           | 233                      | 382                         | 0      | 275                 |

![image](https://github.com/user-attachments/assets/412d1bb9-5515-4626-8436-d36a9994e6ac)

Conclusion: Despite the resource-intensive nature of biometric processing, the system provided an acceptable response time and error-free operation under high load.

### 👤 Results for the page for adding users to the database

| Request Label | Total Requests | Avg. Response Time (ms) | 90% Completed Within (ms) | Errors | Throughput (req/s) |
|---------------|----------------|--------------------------|-----------------------------|--------|---------------------|
| HTTP Request  | 2000           | 99                       | 182                         | 0      | 327.8               |

![image](https://github.com/user-attachments/assets/48a28828-810e-4a1d-871d-2dccd2ac1cac)

Conclusion: The new User registration interface showed the lowest average response time among all modules, which indicates its high performance.

### 📡 Results for the dashboard

| Request Label | Total Requests | Avg. Response Time (ms) | 90% Completed Within (ms) | Errors | Throughput (req/s) |
|---------------|----------------|--------------------------|-----------------------------|--------|---------------------|
| HTTP Request  | 2000           | 165                      | 275                         | 0      | 302.2               |

![image](https://github.com/user-attachments/assets/4c9b2447-06d1-40c5-994f-5a59e17a10cc)

Conclusion: The dashboard consistently handles frequent real-time data updates without failures and with an acceptable delay.

### 🏁 General conclusion based on the test results

The ACS components have shown high performance and stability under load, simulating real-world operation in high-intensity conditions. All modules:

- Successfully processed 2000 requests without errors;

- They showed an average response time in the range of 99-233 ms;

- We provided high throughput (275-327 requests per second).

Thus, the system is able to function effectively in real time with massive simultaneous access, which is critically important for ensuring the security and continuity of business processes at the facility.

## ⚠️ Risks and Assumptions

During load testing, the following assumptions and limitations were defined to ensure the reliability and consistency of the results:

#### 🛠️ Server and database configurations are optimal

It is assumed that the server hosting the Access Control System (ACS), including its web server, backend services, and database engine (e.g., MySQL or PostgreSQL), are properly configured.

- The server has sufficient hardware resources (CPU, RAM, SSD).

- Database indexing and query optimization are implemented.

- No background services interfere with performance.

#### 🧍‍♂️ Virtual users simulate realistic user behavior

Apache JMeter is configured to emulate actual user interactions, including:

- RFID card scans with HTTP POST requests to the server.

- Biometric image uploads and recognition response handling.

- Form submission for new user registration.

- Polling the monitoring dashboard with regular GET requests.

#### 🧪 Testing is performed in an isolated environment

The test environment is detached from external influences to eliminate variability:

- No other applications or network activity compete for bandwidth or CPU.

- Only the test clients and the system under test (SUT) are active.

- The server is not under maintenance or updates during testing.

#### 📏 Network latency is negligible or controlled

All load generators and the SUT reside on the same local network, or a network with minimal latency and packet loss, to ensure accurate measurement of system-level performance.

#### 📦 Static data assumptions

The system is preloaded with a typical number of users and records to reflect a realistic state of operation, but without excessive data growth (e.g., full database, archived logs).

## ✅ Conclusions Based on Load Testing

The results of the load testing provide key insights into the system's ability to handle concurrent users and high transaction volumes. Here's a detailed summary of what was learned:

#### 📊 Component-Level Performance Analysis

- RFID Authentication demonstrated a stable average response time of 177 ms with a maximum (90th percentile) under 288 ms, even with 2000 requests over a 5-second interval.

- Biometric Authentication, being more resource-intensive (due to image processing), had a higher average response time of 233 ms, which remained acceptable.

- User Registration (DB write) operations were the fastest, with an average of 99 ms, indicating efficient backend logic and database interaction.

- The Monitoring Dashboard handled continuous polling with 165 ms average response time, suitable for real-time updates.

#### 🚀 Throughput and Load Tolerance

All modules showed high throughput:

- Up to 327.8 req/sec on the registration interface.

- A consistent 0% error rate, indicating that no requests failed under load.

#### 🔒 Stability and Scalability Confirmed

The ACS performed reliably without crashes, slowdowns, or timeout errors.

- No resource exhaustion (e.g., memory leaks or CPU spikes) was observed.

- Logs and server monitoring indicated healthy operating conditions throughout the test.

#### 📈 Preparedness for Real-World Deployment

The test scenarios confirm the system’s readiness for deployment in environments where dozens or even hundreds of users interact concurrently — such as universities, offices, or industrial facilities.

- The performance under simulated high load implies good scalability potential.

- The system can accommodate growing user bases or peak-hour surges without infrastructure upgrades.

#### 🔧 Future Recommendations

- Conduct extended-duration load tests (e.g., 1-hour stress tests).

- Introduce chaos testing or fault injection to evaluate fault tolerance (e.g., simulate DB failure).

- Monitor metrics like CPU, memory, disk I/O, and network traffic to ensure no hidden bottlenecks exist.

# Authors
If you have any questions, you can ask them to us by writing to us at email:
- ivanovvvvvvvanton3829@gmail.com
