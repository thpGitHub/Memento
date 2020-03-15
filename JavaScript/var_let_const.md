``var``, ``let`` et  ``const`` 
---

|              | Portée    | Réaffectation  | Mutable |
| :----------: | ------    |-----           |:-----                                 |
| var          | fonction  | oui            | oui                                   |
| let          | bloc      | oui            | oui                                   |      
| const        | bloc      | non            | oui (pour les tableaux et les objets) |

L'instruction let permet de déclarer une variable dont la portée est celle du bloc courant;

voir pour var (portée dans une fonction)
a voir aussi le hoisting

> Au niveau le plus haut (la portée globale), let crée une variable globale alors que var ajoute une propriété à l'objet global :

````javascript
    var x = 'global';
    let y = 'global2';

    console.log(this.x); // "global"
    console.log(this.y); // undefined
    console.log(y);      // "global2"
````
---
``var``
---
> Les variables déclarées avec var ont une portée de fonction cad qu'une variable déclarée dans une fonction sera accéssible à tout ce qui se trouve dans cette dernière mais pas depuis l'extérieur.
````javascript
    function myFunction() {
        var myVar = "Nick";
        console.log(myVar); // "Nick" - myVar is accessible inside the function
    }
    console.log(myVar); // Throws a ReferenceError, myVar is not accessible outside the function.
````
````javascript
    function myFunction() {
        var myVar = "Nick";
        if (true) {
            var myVar = "John";
            console.log(myVar); // "John"
        // actually, myVar being function scoped, we just erased the previous myVar value "Nick" for "John"
            }
        console.log(myVar); // "John" - see how the instructions in the if block affected this value
    }
    console.log(myVar); // Throws a ReferenceError, myVar is not accessible outside the function.
````
---
``let``
---
> Les variables déclarées avec let , ne sont pas accessible avant leur 
- ont une portée de bloc
- ne sont pas accessible avant leur affectation
- ne peuvent pas être redélarées dans le même bloc (TDZ : Zone Morte Temporelle)
````javascript
    function myFunction() {
        let myVar = "Nick";
        if (true) {
            let myVar = "John";
            console.log(myVar); // "John"
            // actually, myVar being block scoped, we just created a new variable myVar.
            // this variable is not accessible outside this block and totally independent
            // from the first myVar created !
        }
        console.log(myVar); // "Nick", see how the instructions in the if block DID NOT affect this value
    }
    console.log(myVar); // Throws a ReferenceError, myVar is not accessible outside the function.
````
> Lorsqu'on redéclare une même variable au sein d'une même portée de bloc, cela entraîne une exception SyntaxError.
````javascript
    if (x) {
        let toto;
        let toto; // SyntaxError
    }
````
> Il est possible d'obtenir des erreurs au sein de l'instruction Instructions/switch. En effet, il y a un seul bloc implicite pour cette instruction.

````javascript
    switch (x) {
        case 0:
          let toto;
          break;

        case 1:
          let toto; // SyntaxError for redeclaration.
          break;
    }
````
> si on ajoute une instruction de bloc dans la clause case, cela créera une nouvelle portée et empêchera l'erreur :

````javascript
    let x = 1;

    switch(x) {
      case 0: {
        let toto;
        break;
      }  
      case 1: {
        let toto;
        break;
      }
    }
````
---
``let``
---




---

## Hoisting (remontée de variables) :

> Les déclarations de variables (et les déclarations en général) sont traitées avant que n'importe quel autre code soit exécuté. Ainsi, déclarer une variable n'importe où dans le code équivaut à la déclarer au début de son contexte d'exécution. Cela signifie qu'une variable peut également apparaître dans le code avant d'avoir été déclarée. Ce comportement est appelé « remontée » (hoisting en anglais) car la déclaration de la variable est « remontée » au début de la fonction courante ou du contexte global

````javascript
bla = 2
var bla;
// ...

// est implicitement traité comme :

var bla;
bla = 2;
````