JavaScript Functions
===
* function expression
* function declared
* arrow function


>La différence entre fonction déclaré et une fonction expression est lié à la facon dont le navigateur interprète et stock en mémoire ces informations.
Dans le cas d’une fonction déclarée de manière classique, toute la fonction est chargée dans la mémoire du navigateur même si elle n’est pas utilisée immédiatement. À la différence d'une fonction expression qui ne sera chargée que lorsqu’elle sera appelée.   
La fonction fléchée est un sucre syntaxique (sugar syntax) pour créer une fonction expression anonyme.
 **Elle ne possède pas ses propres valeurs pour this, arguments, super, ou new.target.**    


## function expression

```javascript
    let message = function(){
        alert('Im me');
    }
```
Déclaration et appel : une fonction expression ne peut pas être appellée avant sa déclaration

```javascript
    message();
    /*
    error :
    Uncaught ReferenceError: Cannot access 'expression_function' before initialization
    */    
    let message = function(){
        alert('Im me');
    }
```

## function declared

```javascript
    function message(){
        alert('Im me');
    }
```

Déclaration et appel : une fonction déclarée peut être appellée avant sa déclaration

```javascript
    message();
    // output : Im me
    function message(){
        alert('Im me');
    }
```

## arrow function

```javascript
    let message = () => {
        alert('Im me');
    }
```
