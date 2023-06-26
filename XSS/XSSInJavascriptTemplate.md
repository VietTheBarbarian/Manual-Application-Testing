**XSS in JavaScript template literals**

XSS in JavaScript template literals

When the XSS context is into a JavaScript template literal, there is no need to terminate the literal. Instead, you simply need to use the ${...} syntax to embed a JavaScript expression that will be executed when the literal is processed. For example, if the XSS context is as follows: 
```
<script>
 ... 
var input = `controllable data here`;
 ... 
</script>
``` 
then you can use the following payload to execute JavaScript without terminating the template literal: 
```
${alert(document.domain)}
```

From <https://portswigger.net/web-security/cross-site-scripting/contexts> 
