![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0ce61e7a-3dec-4590-a0f8-3986d0316adf)**Basic ssrf**

Url in parameter to check stock 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/959c1116-c448-4bba-bea5-f7d14eb80fd5)


Changed to localhost with admin directory
```
stockApi=//localhost/admin
```
We can delete user
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9f8f1417-1690-4238-b16f-1893b2fff578)


302 request show we deleted the user
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e120b66c-a691-4579-b658-e5dc5c231f02)

**SSRF with blacklist-based input filter**
Blacklisted input
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7ac2aa19-85d3-400a-a832-f0d794c41da7)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7a0d9bc6-2bfb-48c4-9595-3c0a4421373b)



Bypass blacklist
127.1
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0f76213f-9d4d-485d-b68b-b16d6cf0316f)


Can also converted to decimal 127.0.0.1
https://www.ipaddressguide.com/ip
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b1b5dd6a-1421-40da-a570-c3723d7ebf69)




Block from going into admin 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5bd150ce-6559-4337-8959-afe2b07a50c3)






Bypass by double url encode the "a" in admin
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/029e6707-fc95-4997-a2df-cc3aa4e1647d)



Deleted the user
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c5392c06-a082-4264-9ba1-0bc93e73fde4)

**SSRF with filter bypass via open redirection vulnerability**


Next stock shows a path that can be exploited
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/247bf5db-4bbe-41c7-bdf0-d56707e1fe78)

Change path to google.com
Get 302 request 
Open redirect vuln
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/658f7286-4025-4754-84e2-52d41bc1fe89)


On our stock check 
We can exploit the open redirect from the next product page. Url encode
/product/nextProduct?currentProductId=2&path=/product?productId=3
/product/nextProduct?currentProductId=2&path=http://192.168.0.12:8080/admin
Real world to get the ip to admin you try all the IP
We can now access admin panel via redirection
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7fa44524-8b0c-40b0-819f-0de63e0bfd73)


Delete user via open redirection
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8b1217a6-b9b9-4097-bc87-3e98675af8e3)






