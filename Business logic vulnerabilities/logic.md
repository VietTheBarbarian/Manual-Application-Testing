**Excessive trust in client-side controls**

From <https://0a4400ea042d9b528462379e00f700ab.web-security-academy.net/cart?err=INSUFFICIENT_FUNDS> 

Login and attempt purchase 
Cant 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5423ee6f-5617-4d3e-9585-8549f89f8117)


Price is set when you add something to cart
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b6a2fe05-aaff-4a2d-a0ec-e2b5fdebd31d)


Change price to exploit
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d40d6fd7-d368-4754-83cd-3af0cddf117e)

**High-level logic vulnerability**

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-high-level> 

Changing cart amount to a negative number will give us a negative pricing
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/15d8a625-8a6d-47b2-bdc7-17fcaa619217)


Since the amount is negative we can add another product to make a pricing a positive number to continue checkout
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c5ce8e27-95da-47f6-bfa0-4150f6aef235)


We can now buy 14 furbabies
To solve lab
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bdb342ca-c020-47ee-bfd3-5a012a58b661)

**Low-level logic flaw**

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-low-level> 

Add item to cart
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a6fb495b-e3f6-4313-93cb-c2cad5e5ce06)

Cant add more than 99 it seem
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c60a5fc9-9f66-40c6-b0a0-55db715ac710)


Repeating an absurd amount. Refresh every time
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4f728d4e-b62b-42bd-ad0d-495c4896d9a3)




Look like we exceed the limit resulting in a negative total
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7ec7e408-08c5-4367-b9e4-2ac5bb0e5439)

Add another product and repeat attack to but this time do 323 null payload
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/227244ac-28a2-4b04-b263-5d13ee4f68cb)

Open option an do 1 thread than commit attack
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/204f2d4e-fdd6-4497-89df-b1bf5d5185d1)

Our total now
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8895f4aa-3640-47da-bac5-b60782329e2d)

Add on until we can purchase
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bd2d17d9-4a35-4546-85ff-c69e7c18f08b)

**Inconsistent handling of exceptional input**

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-inconsistent-handling-of-exceptional-input> 

Ask us to use engagement tool but you can really just fuzz directory for this
/admin


• After a short while, look at the "Site map" tab in the dialog. Notice that it discovered the path /admin. 
• Try to browse to /admin. Although you don't have access, an error message indicates that DontWannaCry users do. 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-inconsistent-handling-of-exceptional-input> 

• Try to browse to /admin. Although you don't have access, an error message indicates that DontWannaCry users do. 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-inconsistent-handling-of-exceptional-input> 



Our register reveal please use @dontwannacry.cm email dress
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7d6001fd-6276-4c4e-9fee-c9ee1d616bfb)

Our email client to get email
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/91d37b6f-b4b3-4f21-9df2-78fc18080519)

• Go back to the lab and register with an exceptionally long email address in the format: 
very-long-string@YOUR-EMAIL-ID.web-security-academy.net 
The very-long-string should be at least 200 characters long. 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-inconsistent-handling-of-exceptional-input> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8616de80-272e-49bc-9f48-d1dfeffba949)

very-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-string@exploit-0adb00af04339b96852603b601310005.exploit-server.net



After confirming it showed that our string is truncated to 255 characters
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b7faaa84-da10-4c97-ae71-a3b0f54d8e6f)




• Log out and go back to the account registration page. 
• Register a new account with another long email address, but this time include dontwannacry.com as a subdomain in your email address as follows: 
very-long-string@dontwannacry.com.YOUR-EMAIL-ID.web-security-academy.net 
Make sure that the very-long-string is the right number of characters so that the "m" at the end of @dontwannacry.com is character 255 exactly. 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-inconsistent-handling-of-exceptional-input> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a22c52d2-d19f-4e76-9fd1-94b380605505)


```
very-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-string@dontwannacry.comexploit-0adb00af04339b96852603b601310005.exploit-server.net
```


```
safasfasfsafasfklasfnsfsafsfffvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-stringvery-long-string@dontwannacry.com.exploit-0adb00af04339b96852603b601310005.exploit-server.net
```

We now have access to  admin panel

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cfd5a788-72c2-481f-bd96-ebb7a7980abc)


**Inconsistent security controls**

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-inconsistent-security-controls> 


Same as previous need @dontwannacry.com to access admin
Register
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/964f2589-8502-467f-8325-39327baab51e)


Update to the following 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a57d42da-163e-45da-a18f-4d22c183271e)

Now have admin 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a1cc0e8f-f9d9-4048-b0d8-4f419d9e6daf)

**Weak isolation on dual-use endpoint**

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-weak-isolation-on-dual-use-endpoint> 
Capture a change password request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fefd4f40-097e-4563-98d1-fb2fa01248ce)




you remove the current-password parameter entirely, you are able to successfully change your password without providing your current one. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5b952e16-bb93-4c68-bb27-72faf72fb64c)

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-weak-isolation-on-dual-use-endpoint> 


Changing administrator password
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/35088851-9175-444b-9018-ad627caf8aac)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a4c37bea-bdd0-4590-9879-b01d5437ae99)



**Insufficient workflow validation**

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-insufficient-workflow-validation> 

Adding a item to cart and checking out result in the following request



/cart/order-confirmation?order-confirmed=true
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c1299203-ddc8-48ff-a1b9-383ee01fab27)



To complete the lab add the leather jacket and then send the same request 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f1fba5a9-2125-41ae-8fbb-251844dc4990)



**Authentication bypass via flawed state machine**

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-authentication-bypass-via-flawed-state-machine> 

Logging in we got role selectors
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/00e206c7-a03d-46b1-9a06-9681af098324)



Admin interfaces cant be access and no admin role in role selector
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4c1b6103-568b-4588-b5e4-aedbbe78d2e1)


We can probably bypass this by turning on proxy intercept and dropping role selector
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1fad7618-616f-4d55-b4c2-676d217262c4)


After dropping go to homepage and you should have the admin panel

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2e0d6183-89c4-43ed-9bb4-a1b45a6ec07b)


![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ef8f5227-7c37-43ba-990a-2e01c94c0d07)


**Flawed enforcement of business rules**

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-flawed-enforcement-of-business-rules> 

We got a discount code
NEWCUST5
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/24e90fa8-45f9-4ba7-af83-d58941e65570)


Got another one SIGNUP30 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5eb0ce3d-adcc-4d60-9ca9-e584e4b7e07a)


Cant apply the same coupon twice but we can alternate between the two code
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0ce59ecc-a3d1-4adf-9f02-80e1cb8e8565)



**Infinite money logic flaw**

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-infinite-money> 

log in and sign up for the newsletter to obtain a coupon code, SIGNUP30. Notice that you can buy $10 gift cards and redeem them from the "My account" page. 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-infinite-money> 

Add a gift card to your basket and proceed to the checkout. Apply the coupon code to get a 30% discount. Complete the order and copy the gift card code to your clipboard. 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-infinite-money> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/534998d1-5d94-4fac-b2bf-ce6041d1b606)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/198023f7-b748-4252-bdf6-9245da931b4d)




uW8GAtXn0J

From <https://0af4005c03534e548202303a000900ca.web-security-academy.net/cart/order-confirmation?order-confirmed=true> 

• Go to your account page and redeem the gift card. Observe that this entire process has added $3 to your store credit. Now you need to try and automate this process. 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-infinite-money> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a7a23875-c76f-456a-a7ab-687c6385b130)



• Study the proxy history and notice that you redeem your gift card by supplying the code in the gift-card parameter of the POST /gift-card request. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4b76b4a1-282b-4386-a628-a9898e52fd1f)

• Go to "Project options" > "Sessions". In the "Session handling rules" panel, click "Add". The "Session handling rule editor" dialog opens. 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-infinite-money> 
In the dialog, go to the "Scope" tab. Under "URL Scope", select "Include all URLs". 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-infinite-money> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/036a8171-45ce-457a-aa61-ccb10f3ec98a)



• Select the following sequence of requests: 
```
POST /cart POST /cart/coupon POST /cart/checkout GET /cart/order-confirmation?order-confirmed=true POST /gift-card
``` 

• Then, click "OK". The Macro Editor opens. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f9a2b024-1247-4ca4-9d1d-9fc2b701e09d)

• In the list of requests, select GET /cart/order-confirmation?order-confirmed=true. Click "Configure item". In the dialog that opens, click "Add" to create a custom parameter. Name the parameter gift-card and highlight the gift card code at the bottom of the response. Click "OK" twice to go back to the Macro Editor. 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-infinite-money> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/97a696de-f520-44be-9808-155898858786)


• Select the POST /gift-card request and click "Configure item" again. In the "Parameter handling" section, use the drop-down menus to specify that the gift-card parameter should be derived from the prior response (response 4). Click "OK". 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-infinite-money> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7d42739c-5097-4d96-9ec9-bf8710e8028c)



• In the Macro Editor, click "Test macro". Look at the response to GET /cart/order-confirmation?order-confirmation=true and note the gift card code that was generated. Look at the POST /gift-card request. Make sure that the gift-card parameter matches and confirm that it received a 302 response. Keep clicking "OK" until you get back to the main Burp window. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/24d7d80d-1fa2-4a94-a09d-0d4fae99b8e3)

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-infinite-money> 

• Send the GET /my-account request to Burp Intruder. Use the "Sniper" attack type. 
• On the "Payloads" tab, select the payload type "Null payloads". Under "Payload settings", choose to generate 412 payloads. 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-infinite-money> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/546e05e2-fafd-4f9d-a301-a08babfe0c33)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/11cfedb5-c18a-4781-8041-eccd1ecbcc4d)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/90b1368e-e687-42c7-833e-0b43d181e7e8)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6b8a41fe-116b-4f6b-be9c-359c4c842e04)


Wait until we have enough money
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/388bd6f8-ccb4-4cc5-9fbe-8d752e6acdff)



**Authentication bypass via encryption oracle**

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-authentication-bypass-via-encryption-oracle> 

logic flaw that exposes an encryption oracle to users

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-authentication-bypass-via-encryption-oracle> 

Logg in with the stay logged in option
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6df7b2e2-46ab-4cd7-bd2b-5f54b3f8c52e)


Notice encrypted stay logged in cookie
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/051dd85b-da37-44aa-b82f-2636f1e33d0f)


When submitting a comment without a legit email we also get a encrypted cookie and a 
Invalid email address: v.gmail.com
error message reflects your input from the email 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/12b1d241-d1ff-4d13-89e4-6d9e63d74e2c)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7010b31d-3f38-4504-bb98-51536f193282)






response sets an encrypted notification cookie before redirecting you to the blog post. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/198fec79-e3af-4229-b384-936a7d5858fe)


Deduce that this must be decrypted from the notification cookie. Send the POST /post/comment and the subsequent GET /post?postId=x request (containing the notification cookie) to Burp Repeater. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/96e4aa50-ee58-43b6-8d22-c260e16326a4)



We can decrypt and encrypt with these request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/06e70072-82b4-4109-ba99-e751255b1df9)




Copy and paste the stay-logged-in to the notification
```
eTTFQqcYuS0NvFrn97s8OqflnE5b%2fyo3cDIVE8CHr%2bk%3d;
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/06aa5de3-f0e1-4e2b-a6a8-9b3c9cd8b5f5)


This reveals that the cookie should be in the format username:timestamp. Copy the timestamp to your clipboard. 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-authentication-bypass-via-encryption-oracle> 

```
wiener:1698591385769
```  

Go to the encrypt request and change the email parameter to administrator:your-timestamp. Send the request and then copy the new notification cookie from the response. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/74596961-f45a-4097-9c19-5f7f172bc54d)


Copy and paste the notification cookie into our notification to decrypt
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/699c0fbd-892d-4eb5-b146-ad7adf7d03c0)


In Decoder, URL-decode and Base64-decode the cookie.

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-authentication-bypass-via-encryption-oracle> 

```
1YbFohTmO0FT%2ba0s4kyoq6oJEhRkp6%2f9hkXS9bt88DePbBN3HExqm9KtBh3M1RB%2bdTq%2fPn98gVdITJBS8pNgVQ%3d%3d
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f2c7ff32-c913-4896-822d-1da82810b922)



In Burp Repeater, switch to the message editor's "Hex" tab. Select the first 23 bytes, then right-click and select "Delete selected bytes". 

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-authentication-bypass-via-encryption-oracle> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a43f796c-fa88-4ef5-b573-c264a48746cf)


Than encode as base64 and url
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1414e38d-8234-45e5-b380-0a36967777a6)

Paste final output in the decrypt

We get an error indicating input length must be multiple of 16
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e576eb0e-cda1-4a5d-8970-faaa0d3afe73)


• In Burp Repeater, go back to the encrypt request and add 9 characters to the start of the intended cookie value, for example: 
```
xxxxxxxxxadministrator:your-timestamp
``` 
Encrypt this input and use the decrypt request to test that it can be successfully decrypted. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6ad0d5a0-bd19-46ec-be34-830ee2802eaa)


URL-decode and Base64-decode the cookie

From <https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-authentication-bypass-via-encryption-oracle> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/58e142e6-2c31-44a2-84da-1085495346be)


Delete 32 bytes than base64 encode and url encode

```
%47%69%47%32%56%4f%67%7a%59%39%4a%32%51%50%41%43%41%68%4e%2f%45%52%51%6b%44%58%48%33%55%65%59%76%2f%63%7a%6a%5a%78%78%44%6f%65%4d%3d
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e840d340-f142-46c6-95d0-ba01ec803e00)


Now we can put it on our decrypt 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ebfcf1fd-a560-45cc-b378-faa447a78a6a)


Check the response to confirm that your input was successfully decrypted and, crucially, no longer contains the "Invalid email address: " prefix. You should only see administrator:your-timestamp. 

Copy the cookie notification

Go to burp and turn on proxy intercept
Intercept home
Delete session and replaced stay-logged-in with our decrypt
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/40b32b65-0f8c-498b-aa27-a24404532e54)


Forward and now we have admin panel
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6d3d5165-1f56-4f0a-a56e-94cf95edf1d6)

 
