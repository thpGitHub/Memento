# Object

Les objets n'ont pas de longueur, on peut utiliser `object.keys()` qui nous retourne un tableau des propriétés de l'objet.

````javascript
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};

console.log(Object.keys(object1).length);
// output : 3
````

````javascript
const key = "a"
console.log(object1.key)
// undefined car ici la clé .key n'existe pas dans nobject1
// .key est une variable !

console.log(object1[key])
// somestring
````
