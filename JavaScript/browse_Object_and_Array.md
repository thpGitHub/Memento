for ... in, for ... of, foreach()
===
### 1. for ... in  (instruction)
>(pour les objets, éviter fortement de l'utiliser sur un array !)

  >  permet d'itérer sur les propriétées d'un objet
```javascript
var obj = {a:1, b:2, c:3};
    
for (var prop in obj) {
    console.log(`obj.${prop} = ${obj[prop]}`);
}

// Affiche dans la console :
// "obj.a = 1"
// "obj.b = 2"
// "obj.c = 3"
```
> méthodes utiles :
- Object.values()  // renvoie un tableau contenant les valeurs des propriétés propres énumérables d'un objet
````javascript
const object1 = {
    a: 'somestring',
    b: 42,
    c: false
};

console.log(Object.values(object1));
// expected output: Array ["somestring", 42, false]
````
- Object.keys()  //  renvoie un tableau contenant les noms des propriétés propres à un objet (qui ne sont pas héritées via la chaîne de prototypes) et qui sont énumérables
````javascript
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};

console.log(Object.keys(object1));
// expected output: Array ["a", "b", "c"]

````
### 2. for ... of (instruction)
```javascript
const array1 = ['a', 'b', 'c'];

for (const element of array1) {
    console.log(element);
}

// expected output: "a"
// expected output: "b"
// expected output: "c"
```

### 3. foreach() (methode)
> La méthode forEach() permet d'exécuter une fonction donnée sur chaque élément du tableau.



