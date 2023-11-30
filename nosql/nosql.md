**Detecting NoSQL injection**
• In Burp's browser, access the lab and click on a product category filter. 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-detection> 

Right-click the category filter request and select Send to Repeater.

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-detection> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9a154f38-e0fe-46c1-8898-906e39666871)





submit a ' character in the category parameter. Notice that this causes a JavaScript syntax error. This may indicate that the user input was not filtered or sanitized correctly. 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-detection> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8b996d4b-c5d5-4905-8fde-e1c8fca1471f)



Submit a valid JavaScript payload in the value of the category query parameter. You could use the following payload: 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-detection> 

```
Gifts'+'
```

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-detection> 
Make sure to url encode
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/90301423-5a0f-4def-b50c-151b372f22c7)


Identify whether you can inject boolean conditions to change the response: 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-detection> 

• Insert a false condition in the category parameter. For example: 
```
Gifts' && 0 && 'x
``` 
Make sure to URL-encode the payload. Notice that no products are retrieved. 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-detection> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/33ab1bfa-ac33-4451-9e79-2f1e389609c2)



• Insert a true condition in the category parameter. For example: 
```
Gifts' && 1 && 'x
``` 
Make sure to URL-encode the payload. Notice that products in the Gifts category are retrieved. 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-detection> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ad0e7d40-b777-43b3-80bf-cea2a7c84048)






Submit a boolean condition that always evaluates to true in the category parameter. For example: 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-detection> 

```
Gifts'||1||'
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d599623d-e01a-4118-8095-13021f8b1a6e)

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-detection> 
Copy the URL and load it in Burp's browser. Verify that the response now contains unreleased products. The lab is solved. 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-detection> 





**Exploiting NoSQL operator injection to bypass authentication**

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-bypass-authentication> 

Log in with account given
Send the post/login request to repeater
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/902ce71c-1529-45b2-bc20-deb541696b3f)



In Repeater, test the username and password parameters to determine whether they allow you to inject MongoDB operators: 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-bypass-authentication> 

• Change the value of the username parameter from "wiener" to {"$ne":""}, then send the request. Notice that this enables you to log in.

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-bypass-authentication> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/463afa94-cb4a-47fc-85e5-b3eb5d544fde)



• Change the value of the username parameter from {"$ne":""} to {"$regex":"wien.*"}, then send the request. Notice that you can also log in when using the $regex operator.

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-bypass-authentication> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9af3b38c-08d2-4c66-ab94-c6decfed7c19)

• With the username parameter set to {"$ne":""}, change the value of the password parameter from "peter" to {"$ne":""}, then send the request again. Notice that this causes the query to return an unexpected number of records. This indicates that more than one user has been selected.

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-bypass-authentication> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f7ab8dca-92e2-43b5-bb0e-20b67b528b00)




• With the password parameter set as {"$ne":""}, change the value of the username parameter to {"$regex":"admin.*"}, then send the request again. Notice that this successfully logs you in as the admin user. 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-bypass-authentication> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2e8438ca-9feb-4299-b9a9-1ea022e83405)



Show request in browser
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8e26a6f2-93a4-49d8-87d5-dddf522a699e)


**Exploiting NoSQL injection to extract data**

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 


In Burp's browser, access the lab and log in to the application using the credentials wiener:peter. 
Send to repeater
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bcfb7ce8-c9cd-4467-a6bb-a76635d6f54c)



Submit a ' character in the user parameter. Notice that this causes an error. This may indicate that the user input was not filtered or sanitized correctly. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0b63fb4b-d6c1-4633-aa36-f6b3dc1b929a)


Submit a valid JavaScript payload in the user parameter. For example, you could use wiener'+' 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/eb02f829-4422-44fe-a366-e8f51a3aac7e)

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 
Make sure to URL-encode the payload by highlighting it and using the hotkey Ctrl-U. Notice that it retrieves the account details for the wiener user, which indicates that a form of server-side injection may be occurring. 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 




Identify whether you can inject boolean conditions to change the response: 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 


• Submit a false condition in the user parameter. For example: 
```
wiener' && '1'=='2
``` 
Make sure to URL-encode the payload. Notice that it retrieves the message Could not find user. 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2b56a57f-9008-4821-ac85-a94968b44d66)




• Submit a true condition in the user parameter. For example: 
```
wiener' && '1'=='1
``` 
Make sure to URL-encode the payload. Notice that it no longer causes an error. Instead, it retrieves the account details for the wiener user. This demonstrates that you can trigger different responses for true and false conditions. 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b88d73c9-b76c-4e95-8e9d-aae9aaae9479)





Identify the password length: 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 


• Change the user parameter to
```
administrator' && this.password.length < 30 || 'a'=='b
```
, then send the request. 
Make sure to URL-encode the payload. Notice that the response retrieves the account details for the administrator user. This indicates that the condition is true because the password is less than 30 characters. 

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/230c4f8b-2de1-482f-ae47-26ba82aa5f63)


• Reduce the password length in the payload, then resend the request.
• Continue to try different lengths.
• Notice that when you submit the value 9, you retrieve the account details for the administrator user, but when you submit the value 8, you receive an error message because the condition is false. This indicates that the password is 8 characters long.

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/19cb4bfa-37b9-452e-a922-5e3c28557b9a)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bda97265-4fd9-4fbf-9682-1ac5b7117bd1)



Right-click the request and select Send to Intruder.

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 

In Intruder, enumerate the password: 
Change the user parameter to 
```
administrator' && this.password[§0§]=='§a§
```
. This includes two payload positions. Make sure to URL-encode the payload.

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 

Set the attack type to Cluster bomb

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/32211c62-72db-4278-b939-5a230393d060)





• In the Payloads tab, make sure that Payload set 1 is selected, then add numbers from 0 to 7 for each character of the password.

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/32767ee4-78f1-4304-8a93-0db0acebc522)

• Select Payload set 2, then add lowercase letters from a to z. If you're using Burp Suite Professional, you can use the built-in a-z list.

From <https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-data> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/80c32077-cc08-47e2-86be-c90f11dddb9a)





Our password
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e29a0e4a-6495-40aa-9924-1b12fe78d6f8)


