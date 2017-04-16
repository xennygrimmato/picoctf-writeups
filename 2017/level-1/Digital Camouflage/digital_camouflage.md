# Digital Camouflage

Problem statement:

```
We need to gain access to some routers.
Let's try and see if we can find the password in the captured network data: data.pcap.
```

PCAP file: https://webshell2017.picoctf.com/static/8e3a439b323b2bc3cd09702161de2cca/data.pcap

## Solution

After opening the PCAP file in Wireshark, I first exported all HTTP objects
to see what requests were being made.

In the hex payload of `main.html`, I noticed a `D.userid` field.

On following the HTTP stream of `main.html`, I found this:

```
POST /pages/main.html HTTP/1.1
Referer: 10.0.0.1:8080/index.html
User-Agent: User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:44.0) Gecko/20100101 Firefox/44.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9*/*;q=0.8
Host: 10.0.0.1:8080
Connection: Keep-Alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 41
Accept-Language: en-US,en;q=0.5

userid=randled&pswrd=OFBGRW8wdHRIUQ%3D%3DHTTP/1.0 200 OK
Server: BaseHTTP/0.3 Python/2.7.9
Date: Sat, 19 Mar 2016 01:45:28 GMT
Content-type: text/html
```

`pswrd=OFBGRW8wdHRIUQ%3D%3D` clearly indicated that I had almost gotten
the flag.

`%3D` decodes to `=`, which means the password was Base64 encoded.

On decoding the password, we get the following flag: `8PFEo0ttHQ`
