**DOM XSS in document.write sink using source location.search inside a select element**

Our script: Select from arrays and if that array is equal to the select display that
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/53198bc6-f118-4a9c-accd-bdc65a7e56a3)


Our payload
```
https://0af9006b044cfc56812f73a3002000ca.web-security-academy.net/product?productId=14&storeId=viet
```

We added a new productid to our array
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/87305e15-7609-450d-89db-8ead037fa886)





To break out of the select element by closing the select tag 
```
</select>
```

Than include a
```
viet</select><img src ="1" onerror="alert()">
```
