**Reflected XSS into HTML context with most tags and attributes blocked**

Entering
```
<img src=1 onerror=print()>
```

From <https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-most-tags-and-attributes-blocked> 

Result in the following. WAF in place
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6f9d4a3d-b16a-4e6d-a44a-a94cb0652446)


Using Burp Intruder to test which tags and attributes are being blocked 
Intercept on intruder
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/21486e55-1aa1-441b-ba97-439e2532d0f0)

Replaced with 
`*<§§>*`
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0bd5648e-54f2-4382-8a02-ca6fbf03f05d)

From <https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-most-tags-and-attributes-blocked> 



Visit https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-most-tags-and-attributes-blocked 
Copy events to clipboard and paste to use as our payload

Attack finished. Revealed a 200 response for the body payload
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/174b8616-c4b7-4ff1-833f-1967e97c6fa8)




• Go back to the Positions tab in Burp Intruder and replace your search term with: 
```
<body%20=1>
``` 

From <https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-most-tags-and-attributes-blocked> 
• Place the cursor before the = character and click "Add §" twice, to create a payload position. The value of the search term should now look like: `<body%20§§=1>` 

From <https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-most-tags-and-attributes-blocked> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/467ef854-3613-49ff-a86b-48c71eca3138)





• Visit the XSS cheat sheet and click "copy events to clipboard". For payloads 

From <https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-most-tags-and-attributes-blocked> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/45507ad4-8920-4d89-9aea-114c4c2b98aa)



Execute attack
We got 200 request for the following
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cb8caae9-fa08-48af-ac26-2eace21321c5)


In our exploit server we will use onresize 
Our exploit
Url decode
`<iframesrc="https://YOUR-LAB-ID.web-security-academy.net/?search="><body onresize=print()>"onload=this.style.width='100px'>`

Url encode
 `<iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>`
Send 

