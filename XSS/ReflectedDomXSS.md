**Reflected DOM XSS**

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval#examples

Eval security risk. Never use eval

Default it will escape everything after 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/21da98ec-8f27-4907-be87-99ea2cb52775)


To escape we add another `\` which `\\` is used to escape js
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f6a2e9f9-93d4-4f00-9fce-ca2f87a2fe0f)





We havnt fully escape we still have a trailing }
So we add another `}` to end the  `{}` and add `//` to comment out 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1bd61dcd-6407-4802-9316-1fcc520a3096)


Enter payload into our search or url encode and copy and paste
`viet\"-alert()}//`
`https://0a9c0071047f210e816e9df800210010.web-security-academy.net/?search=viet%5C%22-alert%28%29%7D%2F%2`F

`Eval()` takes a string and attempts to run it as Javascript code.

From <https://medium.com/@eric_lum/the-dangerous-world-of-javascripts-eval-and-encoded-strings-96fd902af2bd> 
