# D-Link DCS931L  Command Execution
Affected Product: D-Link DCS930L

Affected Firmware Versions: v1.0.0-v1.13.0

Vulnerability Type: Command Execution



# Vulnerability Description
A command injection vulnerability exists in the setSystemAdmin function of the alphapd binary in D-Link DCS-931L firmware v1.0.0-1.13.0.   
The AdminID parameter is directly taken from user input and inserted into shell commands without proper sanitization, allowing remote attackers to execute arbitrary OS commands via crafted requests.

# Vulnerability Details
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770376421911-ee8eaa5d-5c16-4daa-8881-c8032aa856a4.png)

<font style="color:rgb(31, 35, 40);">In the handler of the binary, the input is first checked in
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770376471046-a1575adf-cb73-4095-960b-fca98c814a02.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770376643865-9b16434f-f9a9-4443-8dd1-1dbe56699dc5.png)<font style="color:rgb(31, 35, 40);">Then, the function further checks . However, only cleans up HTML whitespace characters in the input string and limits the input length to a minimum of 1 and a maximum of 12, without restricting the content.

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770376705130-ff3f7dcf-f7b0-4683-8b9b-aaff83c95132.png)

<font style="color:rgb(31, 35, 40);">The value comes directly from attacker-controlled input. Because the input is not properly restricted, it is directly inserted into the shell command, which allows the attacker to construct an that triggers arbitrary command execution.</font>

# <font style="color:rgb(31, 35, 40);">POC</font>
<font style="color:rgb(31, 35, 40);"></font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770376791345-42cecfb5-2a59-458c-b6d1-b7a130860932.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/58401686/1770376777391-dc15bc12-c090-400f-bc58-b21bcc422627.png)

