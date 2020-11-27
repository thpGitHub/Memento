Callback function
-
> Nous pouvons passer des fonctions en tant que paramètres à d'autres fonctions et les appeler à l'intérieur des fonctions externes.
> En javascript les fonctions sont des objets et nous pouvons donc passer une fonction en paramètre d'une autre fonction
````javascript
    function print(callback) {  
        callback();
    }
````
ici la fonction ``print()`` prend une autre fonction comme paramètre et l'appelle.

---
````javascript
    function greeting(name) {
      alert('Hello ' + name);
    }
    
    function processUserInput(callback) {
      var name = prompt('Please enter your name.');
      callback(name);
    }
    
    processUserInput(greeting);
````
---
- Exemple avec la methode native Javascript : ``setTimeout`` :
````javascript
// avec une fonction nommée :
    const message = function() {  
        console.log("This message is shown after 3 seconds");
    }
     
    setTimeout(message, 3000);
````
````javascript
// avec une fonction anonyme :
    setTimeout(function() {  
        console.log("This message is shown after 3 seconds");
    }, 3000);
````
````javascript
// avec une fonction flèche :
    setTimeout(() => { 
        console.log("This message is shown after 3 seconds");
    }, 3000);
````

Exemple avec un appel simple AJAX et une fonction callback (success)
````javascript
    let get = (url, success) => {
        let xhr = new XMLHttpRequest();
        xhr.onreadystatechange = function () {
            if (this.readyState === 4) {
                success(xhr.responseText);
            }
        };
        xhr.open('GET', url);
        xhr.send();
    };
    
    let getPost = () => {
        get('https://jsonplaceholder.typicode.com/users', function(success){
            let user = JSON.parse(success);
            console.log(user[0].address.city);
        });
    };
    
    getPost();
````
Avec deux fonctions callback (success et error)
````javascript
    let get = (url, success, error) => {
        let xhr = new XMLHttpRequest();
        xhr.onreadystatechange = function () {
            if (xhr.readyState === 4) {
                if (xhr.status === 200) {
                    success(xhr.responseText);
                }else {
                    error(xhr)
                }
            }
        };
        xhr.open('GET', url);
        xhr.send();
    };
    
    let getPost = () => {
        get('https://jsonplaceholder.typicode.com/users', function(success){
            let user = JSON.parse(success);
            console.log(user[0].address.city);
        }, function (error) {
            console.log(error);
        });
    };
    
    getPost();

````