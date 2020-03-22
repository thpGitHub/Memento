JavaScript Functions
===
* function expression
* function declared
* arrow function


>La différence entre fonction déclaré et une fonction expression est lié à la facon dont le navigateur interprète et stock en mémoire ces informations.
Dans le cas d’une fonction déclarée de manière classique, toute la fonction est chargée dans la mémoire du navigateur même si elle n’est pas utilisée immédiatement. À la différence d'une fonction expression qui ne sera chargée que lorsqu’elle sera appelée.   
La fonction fléchée est un sucre syntaxique (sugar syntax) pour créer une fonction expression anonyme.
 **Elle ne possède pas ses propres valeurs pour this, arguments, super, ou new.target.**    

> Les fonctions (à l'exception des fonctions fléchées) ont deux pseudo-paramètres: this et arguments.
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
Les fonctions flèchées sont un sucre syntaxique (sugar syntax) pour créer des expressions de fonction


```javascript
    let message = () => {
        alert('Im me');
    }
```
> Un des bénéfices des fonctions fléchées est que this est bindé au contexte dans lequel la fonction est définie, et non au this de la fonction elle-même
> Une fonction flèchée ne crée pas de nouveau this elle le récupère de son environement.

Avant les fonctions flèchées ont devait utiliser l'astuce that = this ou self = this.
````javascript
    function myFunc() {
        this.myVar = 0;
        var that = this; // that = this trick
        setTimeout(
            function() { // A new *this* is created in this function scope
                that.myVar++;
                console.log(that.myVar) // 1
                
                console.log(this.myVar) // undefined -- see function declaration above
            },
            0
        );
    }
````
Avec les fonctions flèchées
````javascript
    function myFunc() {
        this.myVar = 0;
        setTimeout(
            () => { // this taken from surrounding, meaning myFunc here
                this.myVar++;
                console.log(this.myVar) // 1
            },
            0
        );
    }
````