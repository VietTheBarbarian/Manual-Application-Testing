**Limit overrun race conditions**

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-limit-overrun> 

We are given a promocode 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7e2b0fe1-e57f-4591-a7d7-d0bc7e8f23a5)


Log in and add something to your cart
Than apply the promocode
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3232b292-66d6-4f62-a424-0292c1aebe33)



In Repeater, try sending the GET /cart request both with and without your session cookie. Confirm that without the session cookie, you can only access an empty cart. From this, you can infer that:
	• The state of the cart is stored server-side in your session.
	• Any operations on the cart are keyed on your session ID or the associated user ID.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8166df9a-1fc2-489e-be6a-95fd7ac1d079)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/74c8122f-6985-4b33-8019-556ceb51ea8d)

	
	
	
	
	
	Remove discount code from cart
	Than send to repeater 20 times in a new tab group(ctrl+r)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/917ee785-ac57-4768-9330-1fd1f6dffdbb)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1db990bd-1cc3-4f7c-a545-b42aa2a7ec38)

	
	
	
	
	
	Send the group of requests in sequence, using separate connections to reduce the chance of interference
	
	Observe that the first response confirms that the discount was successfully applied, but the rest of the responses consistently reject the code with the same Coupon already applied message
	
	Remove the discount code from your cart.
	
	In Repeater, send the group of requests again, but this time in parallel, effectively applying the discount code multiple times at once
 
	![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/83f197b3-51ce-43a4-abb3-50dfaeef2d03)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e4ecf924-65a0-4918-b605-ecbfe1286773)

	
	
	
	
	Now  have enough to purchase
	![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/111fe42e-9287-4bb6-9389-d577721c4d88)

	
	
	**Bypassing rate limits via race conditions**
	
	From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-bypassing-rate-limits> 
	
	
	We get lockout after 3 incorrect password attempts
	logging in using another arbitrary username and observe that you see the normal Invalid username or password message. This indicates that the rate limit is enforced per-username rather than per-session.
	
	Deduce that the number of failed attempts per username must be stored server-side.
	
	• Consider that there may be a race window between:
		• When you submit the login attempt.
		• When the website increments the counter for the number of failed login attempts associated with a particular username.
		
Send 20 login request to repeater
Put them in group and send them in parallel
• Infer that if you're quick enough, you're able to submit more than three login attempts before the account lock is triggered.

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/60b6ca44-6014-4492-80e5-591c75ff5e21)






Our exploit 
```
def queueRequests(target, wordlists):

    # as the target supports HTTP/2, use engine=Engine.BURP2 and concurrentConnections=1 for a single-packet attack
    engine = RequestEngine(endpoint=target.endpoint,
                           concurrentConnections=1,
                           engine=Engine.BURP2
                           )
    
    # assign the list of candidate passwords from your clipboard
    passwords = wordlists.clipboard
    
    # queue a login request using each password from the wordlist
    # the 'gate' argument withholds the final part of each request until engine.openGate() is invoked
    for password in passwords:
        engine.queue(target.req, password, gate='1')
    
    # once every request has been queued
    # invoke engine.openGate() to send all requests in the given gate simultaneously
    engine.openGate('1')


def handleResponse(req, interesting):
    table.add(req)
```



Highlight password and send to turbo intruder
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5c5b4a8f-1ad9-43ff-8611-8936600608a3)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/49c35582-2658-45f4-bff3-57bad2de2d64)




From the drop-down menu, select the examples/race-single-packet-attack.py template

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-bypassing-rate-limits> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/75fa5d10-a1be-43b2-876f-0ff87c5f1f32)




Copy and paste exploit
Note that we're assigning the password list from the clipboard by referencing wordlists.clipboard. Copy the list of candidate passwords to your clipboard.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bd7f6d98-0074-4992-bb20-df040480c223)




Attack Password FOUND
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/40b5d263-a873-4562-b13b-a17561b16efe)





**Multi-endpoint race conditions**

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-multi-endpoint> 
Using step recommended to find race condition

Predict potential Collision 
Add a item to your cart
 ![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d605e082-1aba-485e-983c-f59e406147dc)


Than log in and check cart
Empty. 
You can only access an empty cart. From this, you can infer that:
	• The state of the cart is stored server-side in your session.
	• Any operations on the cart are keyed on your session ID or the associated user ID.

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-multi-endpoint> 

indicates that there is potential for a collision.

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-multi-endpoint> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/75717b52-7fd5-445c-9e72-5cc597876cb9)





Benchmark

Add gift card to cart and checkout
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b3184178-408e-4dfb-bf82-7e6f435f9fda)



Go to burpe 
Send the post cart request and post checkout to repeater
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8fb125b3-934c-48c0-8037-b4363b491094)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e736576b-f903-4da2-9575-98afbbc11287)




Send these two request to group
Send group in single connection
Notice from the response times that the first request consistently takes significantly longer than the second one
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c6596cd8-70ac-453e-ac28-5c058b227ed1)


Add a GET request for the homepage to the start of your tab group.
Send all three requests in sequence over a single connection. Observe that the first request still takes longer, but by "warming" the connection in this way, the second and third requests are now completed within a much smaller window
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ae86cf00-97d3-490b-aa82-03d025aa9b6c)


Add a jacket to your cart and send repeater request again
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1f2563b7-ab7f-4d7a-9e51-c8a5bbae7334)


order is rejected due to insufficient funds, as you would expect

Prove the concept
modify the POST /cart request in your tab group so that the productId parameter is set to 1, that is, the ID of the Lightweight L33t Leather Jacket.

Remove the jacket from your cart and add another gift card.

Send request in parallel 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c6e85670-9771-419b-a9f1-241f46ecd70c)

• Look at the response to the POST /cart/checkout request:
	• If you received the same "insufficient funds" response, remove the jacket from your cart and repeat the attack. This may take several attempts.
	• If you received a 200 response, check whether you successfully purchased the leather jacket. If so, the lab is solved.


**Single-endpoint race conditions**

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-single-endpoint> 

carlos@ginandjuice.shop has a pending invite to be an administrator for the site, but they have not yet created an account. Therefore, any user who successfully claims this address will automatically inherit admin privileges. 

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-single-endpoint> 

Log in and change email
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2f896400-4526-42a6-b164-c88dd227c6a4)



Send repeater twice
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c3a35e81-55a6-442d-8c7a-c66284130fa6)



Add to group and change second request to carlos@ginandjuice.shop
Send group in parallel 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2102e198-dc57-4fce-9781-c9b417c00364)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1bfc57e4-e2d0-4923-bb5d-0c10d54a6191)





Confirm
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f7932437-770c-4064-9d2a-dd31a31dc97b)



Refresh
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f1d389e6-5a0a-4748-959e-798f3a640253)



**Partial construction race conditions**

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-partial-construction> 

You must do this one fast cause this is a "race" condition
race condition enables you to bypass email verification and register with an arbitrary email address that you do not own. 

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-partial-construction> 


Register account 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f4e2c3ac-b7cb-49df-bdd0-afe1f76ff00e)


In Burp, from the proxy history, notice that there is a request to fetch /resources/static/users.js.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4c6d2c3e-dbfd-4424-b64c-2f5cb018edc4)


From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-partial-construction> 


Study the JavaScript and notice that this dynamically generates a form for the confirmation page, which is presumably linked from the confirmation email. This leaks the fact that the final confirmation is submitted via a POST request to /confirm, with the token provided in the query string

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-partial-construction> 


In Burp Repeater, create an equivalent request to what your browser might send when clicking the confirmation link. For example:

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-partial-construction> 

```
POST /confirm?token=1 HTTP/2
    Host: YOUR-LAB-ID.web-security-academy.net
    Content-Type: x-www-form-urlencoded
    Content-Length: 0
```



Experiment with the token parameter in your newly crafted confirmation request. Observe that:
	○ If you submit an arbitrary token, you receive an Incorrect token: <YOUR-TOKEN> response.
	○ If you remove the parameter altogether, you receive a Missing parameter: token response.
	○ If you submit an empty token parameter, you receive a Forbidden response.
	
	
Normal
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ad478a8d-069a-4d36-a08a-97b771e733a9)



Experiment with different ways of submitting a token parameter with a value equivalent to null. For example, some frameworks let you to pass an empty array as follows:

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-partial-construction> 
Observe that this time, instead of the Forbidden response, you receive an Invalid token: Array response. This shows that you've successfully passed in an empty array, which could potentially match an uninitialized registration token.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/04378c0e-78ad-4704-9587-701828e4d9a8)

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-partial-construction> 





Send the register to turbo intruder  with username as parameter
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d0acd2de-cca6-4e2d-9279-e2c3fac268bf)



Our exploit
```
def queueRequests(target, wordlists):

    engine = RequestEngine(endpoint=target.endpoint,
                            concurrentConnections=1,
                            engine=Engine.BURP2
                            )
    
    confirmationReq = '''POST /confirm?token[]= HTTP/2
Host: YOUR-LAB-ID.web-security-academy.net
Cookie: phpsessionid=YOUR-SESSION-TOKEN
Content-Length: 0

'''
    for attempt in range(20):
        currentAttempt = str(attempt)
        username = 'User' + currentAttempt
    
        # queue a single registration request
        engine.queue(target.req, username, gate=currentAttempt)
        
        # queue 50 confirmation requests - note that this will probably sent in two separate packets
        for i in range(50):
            engine.queue(confirmationReq, gate=currentAttempt)
        
        # send all the queued requests for this attempt
        engine.openGate(currentAttempt)

def handleResponse(req, interesting):
    table.add(req)
```


From the drop-down menu, select the examples/race-single-packet-attack.py template.
Paste our exploit
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2e587d7a-1a98-41ed-8602-085408aaeb01)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4a4d62ff-bf09-4aba-9148-a690db4e8661)


Will inject 20 user let change our password to something stronger
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5fdbd9b2-6283-4575-a08a-ec7c1004d84c)



Successful 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5e6809c1-f82f-403c-a70e-e617941aa7d8)



**Exploiting time-sensitive vulnerabilities**

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-exploiting-time-sensitive-vulnerabilities> 
contains a password reset mechanism. Although it doesn't contain a race condition, you can exploit the mechanism's broken cryptography by sending carefully timed requests. 

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-exploiting-time-sensitive-vulnerabilities> 

Send the POST /forgot-password request to Burp Repeater and send request to repeater

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-exploiting-time-sensitive-vulnerabilities> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3a366329-7aa3-439c-ba82-55b145850f35)


Observe that every reset request results in a link with a different token.

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-exploiting-time-sensitive-vulnerabilities> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1cb23911-f14d-49bf-b557-2d2b7370704d)



• Consider the following:
	• The token is of a consistent length. This suggests that it's either a randomly generated string with a fixed number of characters, or could be a hash of some unknown data, which may be predictable.
	• The fact that the token is different each time indicates that, if it is in fact a hash digest, it must contain some kind of internal state, such as an RNG, a counter, or a timestamp.

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-exploiting-time-sensitive-vulnerabilities> 


Duplicate the Repeater tab and add both tabs to a new group

Send the pair of reset requests in parallel a few times

• Observe that there is still a significant delay between each response and that you still get a different token in each confirmation email. Infer that your requests are still being processed in sequence rather than concurrently.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cd2e3db4-72f4-4044-8d99-a3165b4ea5db)




**Bypass the per-session locking restriction**

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-exploiting-time-sensitive-vulnerabilities> 

Notice that your session cookie suggests that the website uses a PHP back-end. This could mean that the server only processes one request at a time per session.

Send the GET /forgot-password request to Burp Repeater, remove the session cookie from the request, then send it.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/29f7c5b3-8185-40c2-bebe-6336d0f2a44d)



• From the response, copy the newly issued session cookie and CSRF token and use them to replace the respective values in one of the two POST /forgot-password requests. You now have a pair of password reset requests from two different sessions.

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-exploiting-time-sensitive-vulnerabilities> 
Send the two POST requests in parallel a few times and observe that the processing times are now much more closely aligned, and sometimes identical.

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-exploiting-time-sensitive-vulnerabilities> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/48a69db8-c2f8-42ce-8327-823e921633d5)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/123bc0fe-c830-4807-a5e2-83aee5da7197)



• Go back to your inbox and notice that when the response times match for the pair of reset requests, this results in two confirmation emails that use an identical token. This confirms that a timestamp must be one of the inputs for the hash.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e57a134d-0d3f-44e4-ab79-6cce7a36393c)



• In Repeater, go to the pair of POST /forgot-password requests and change the username parameter in one of them to carlos.
• Resend the two requests in parallel. If the attack worked, both users should be assigned the same reset token, although you won't be able to see this.

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-exploiting-time-sensitive-vulnerabilities> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/72d4ab40-a826-4a56-9ec3-1d79ea2e0390)





• Check your inbox again and observe that, this time, you've only received one new confirmation email. Infer that the other email, hopefully containing the same token, has been sent to Carlos.

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-exploiting-time-sensitive-vulnerabilities> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9badeacf-fbde-4842-b6c3-449de48662f0)




• Copy the link from the email and change the username in the query string to carlos.

From <https://portswigger.net/web-security/race-conditions/lab-race-conditions-exploiting-time-sensitive-vulnerabilities> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e7eb370f-a569-44a8-b6b0-95a1c218832d)


Log in
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0132bd54-9858-4c70-9f1e-791d5ff696e2)

