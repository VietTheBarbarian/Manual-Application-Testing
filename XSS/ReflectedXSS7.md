**Reflected XSS in canonical link tag**

Check to see if anything reflects  with our arbitrary value 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bbbbbbb6-2e0b-47ab-b160-5b13e65d38a8)



Our payload 
```
'?onclick=alert(1)
```
We escaped out but there a trailing '
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/351e8099-a1d6-422a-9f53-df155b962f3e)


Add  `'` to end trail 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d4307088-60d4-4105-b007-542f2fbf0b3f)





We got a event handler but we can't interact with it and not loading onto the page anywhere
Used access key for keyboard shortcut
Browser and OS determine access key 
```
'accesskey='x'onclick='alert(1)
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3aa6ea1b-6ea7-4242-a1a8-496e6ce96067)


We can trigger it with alt shift x
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d718550e-01b5-410d-98b3-2cc52f47b266)


