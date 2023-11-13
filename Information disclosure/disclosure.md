**Information disclosure in error messages**

From <https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-in-error-messages> 

Basic just add some random to a parameter to get error

https://0aa10028032d399c8107663300b400b0.web-security-academy.net/product?productId=%22example%22
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9e348cc5-ba1b-4737-8927-7b25f986e1b7)






**Information disclosure on debug page**

From <https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-on-debug-page> 

Debug page found on cgi
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e4044aed-e43a-4372-bfc4-5c4d5fbdb45b)


Secret key found
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/02bda823-4b44-4ef6-8fd3-3a2e2bfc9052)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b1134795-422d-4c7a-8e0b-4c760b528976)










***Source code disclosure via backup files***

From <https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-via-backup-files> 


Robots.txt reveal additional directory 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/432da097-2097-4dc5-ba17-d3377f295fb5)






Postgres password found
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1baf331e-689e-4f29-8bb1-7cf53aee1ba9)





**Authentication bypass via information disclosure**

From <https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-authentication-bypass> 

Admin only available to local user 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/99daf136-b969-46be-ab5d-a6cb1dbda268)




Send the request again, but this time use the TRACE method: 

From <https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-authentication-bypass> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1fb88321-6ab2-4f6e-9937-956beb1ce1e1)







Notice that the X-Custom-IP-Authorization header, containing your IP address, was automatically appended to your request. This is used to determine whether or not the request came from the localhost IP address. 

From <https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-authentication-bypass> 


• Go to "Proxy" > "Options", scroll down to the "Match and Replace" section, and click "Add". Leave the match condition blank, but in the "Replace" field, enter: 
```
X-Custom-IP-Authorization: 127.0.0.1
``` 
Burp Proxy will now add this header to every request you send. 

From <https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-authentication-bypass> 

Now have admin panel
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c9a740d8-c910-4925-9ecf-b920eed44baf)



Information disclosure in version control history

From <https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-in-version-control-history> 


.git directory found 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/522956a4-e713-4c76-b61e-a382e935f7a0)

Recursive get to get everything
```
wget -r https://0ae200270311210a82ebcf7000000027.web-security-academy.net/.git
```

Use git cola to open up .git downloaded and undo last commit to get password 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/69b4259b-af66-4ba3-b8c9-cdbdba17fa8f)






