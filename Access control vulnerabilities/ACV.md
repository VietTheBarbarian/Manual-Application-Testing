**Access control vulnerabilities**

**Unprotected admin functionality**

From <https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality> 


Disallow in robots.txt
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/754d8c83-3087-4a29-be91-f6b4bae6e233)



Going to admin panel
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3d039c6c-366b-45bd-a3e9-c8b00c36391a)




**Unprotected admin functionality with unpredictable URL**

From <https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url> 

Using curl this time to grep out anything admin
Admin panel here
 
```
└─$ curl https://0a5c00f104768a7a8b7dc37400d600b2.web-security-academy.net/ | grep admin
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 11058  100 11058    0     0  22080      0 --:--:-- --:--:-- --:--:-- 22116
        <title>Unprotected admin functionality with unpredictable URL</title>
                            <h2>Unprotected admin functionality with unpredictable URL</h2>
                            <a class=link-back href='https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url'>
   var adminPanelTag = document.createElement('a');
   adminPanelTag.setAttribute('href', '/admin-ti1qu9');
   adminPanelTag.innerText = 'Admin panel';
   topLinksTag.append(adminPanelTag);
```
                                           



**Unprotected admin functionality with unpredictable URL**

From <https://0a5c00f104768a7a8b7dc37400d600b2.web-security-academy.net/> 


Login with account given

Go to /admin
Look at cookieeditor extension and set Admin to true
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6a3d58e7-03df-4f1a-8e4e-036ad41b8bfb)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5659dc4c-0f16-44dd-8a79-db0803c701ed)








User role can be modified in user profile

From <https://portswigger.net/web-security/access-control/lab-user-role-can-be-modified-in-user-profile> 


Request to repeater. Change email to your email 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2242c3d7-d555-4231-a2d7-c4186ddadd88)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/01a87373-5dd4-4aac-a9f2-8081d7de2fc3)




Change role to 2
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/437b81a0-48a7-462b-8773-59f9c885ae35)



Now have admin panel
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3663161d-6854-403c-8f60-d87f8e2d3bc9)

**User ID controlled by request parameter**

From <https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter> 

Just changed id parameter to another user 
https://0a45002704eae8e483ce69b000530006.web-security-academy.net/my-account?id=carlos


**User ID controlled by request parameter, with unpredictable user IDs** 

From <https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-unpredictable-user-ids> 

Find an account you want this case carlos
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6a3f6b00-4344-41c0-82c5-b087c1b41e35)

Open carlos name 
Got UID 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/93dad9da-0f2d-4881-a600-b3beb3e8315c)

Find user account and paste in parameter
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9fb007e4-07d4-413d-9054-0e4216c04c54)

**User ID controlled by request parameter with data leakage in redirect** 

From <https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-data-leakage-in-redirect> 

When changing parameter to Carlos you can see the api key leak
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1a47c7a1-33f1-490a-8563-270931b63952)



**User ID controlled by request parameter with password disclosure**

Initial login
```
https://0a3d008e036aa8878445465400f200a5.web-security-academy.net/my-account?id=wiener
```

Change parameter to administrator 
```
https://0a3d008e036aa8878445465400f200a5.web-security-academy.net/my-account?id=administrator
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ab6265ea-e23a-4516-9ac4-a78c981b9323)


Password disclosure
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b1fc3885-3637-4bce-b91b-485e718f043e)


Log in and delete


***Insecure direct object references***

Download transcript and intercept request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1df322a1-c09d-4faf-8526-9acd494578e7)


Change 5.txt to 1.txt in our repeater and send request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/210f7a37-4efc-4144-9d30-36ad8f01e6eb)



**URL-based access control can be circumvented**

From <https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented> 


/admin is denied
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cc29eed7-9bfe-4fc9-bcda-dbe50b792481)


Changed X-Original-Url: /random 
Not found.
This indicates that the back-end system is processing the URL from the X-Original-URL header. 

From <https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a7a020c0-5db6-4f96-a1c7-5870d0837ee9)





Changed X-Original-Url: /admin
We got access to admin panel
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d47eeede-0594-4cf1-bc02-7fd9ae0514b9)




To delete we gotta be a little weird with `X-Original-Url: /admin/delete`
And /`?username=carlos`
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/dc363cde-6aeb-4a14-8b5f-b9eb059308fe)


**Method-based access control can be circumvented**

From <https://portswigger.net/web-security/access-control/lab-method-based-access-control-can-be-circumvented> 

Sign into administrator and access admin panel
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cf424a9b-3ce2-406d-9750-df4a1d906baa)

Capture upgrade user request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d4241614-2628-4ad4-aaf5-6c81ee918e60)


Sign into other account with low level access and capture request 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c1e379ff-87bf-4af6-8c58-b354ca8c2704)







Change request to get and enter username to upgrade
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e9146571-8f32-4e37-a0ac-65bd719f7080)






Copy cookie session from other account into upgraded get 
We solved lab
Upgraded our low level access request with cookie of low level 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/660f31fa-2dcc-419a-8eb9-b7286a063152)







**Multi-step process with no access control on one step** 

From <https://portswigger.net/web-security/access-control/lab-multi-step-process-with-no-access-control-on-one-step> 

Log in with administrator and upgrade 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bbcabb6f-c589-47d3-aaa1-cb4ff3851d68)


Copy session cookie from non priv user and paste it into request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d7b6e597-16a3-4c54-8c90-7456b83bb3c0)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0bc1ae1d-94af-4f15-b4b5-6eb174d1aab1)








**Referer-based access control** 

From <https://0afd005903acef5e8183b11600ed00f8.web-security-academy.net/my-account?id=wiener> 

Same thing as previous just changed parameter in get request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5ded2a24-5277-48ec-a1c6-035d6a462bf7)


