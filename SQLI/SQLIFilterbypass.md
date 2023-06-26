SQL injection with filter bypass via XML encoding 
Not paramerized so we can exploit it
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3cf43b61-63ff-4979-a37f-89e693af8bbf)








We got a WAF we can use hackvertor extension 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/47fa2127-bcd9-4c1c-8085-5f82dbd933dc)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6d231d4c-1052-4351-a1f3-f982d0029381)











Highlight our sqli and encode it using hex_entitites
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6014f4ea-4df1-4eaa-8aba-81af2b838af7)


![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/beef339c-f3af-4af2-9a81-6348155ec602)





No long say attack detected 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fb26e03e-5911-4026-bee1-6a8e7bf2b77a)




Getting another column using another null so we can only output 1 column
We need to concatenate if we want more than one column 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/be421a64-b8fe-4a88-9347-32a229626664)



```
1 union select username || '-' || password from users
```
We got username and password
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/dfb30763-347c-4e25-bc70-cd52b7a33076)


