# Object

Les objets n'ont pas de longueur, on peut utiliser `object.keys()` qui nous retourne un tableau des propriétés de l'objet.

````javascript
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
}

console.log(Object.keys(object1).length)
// output : 3
````

````javascript
const key = "a"
console.log(object1.key)
// undefined car ici la clé .key n'existe pas dans object1
// .key est une variable !

console.log(object1[key])
// somestring

console.log(object1["a"]) // somestring
console.log(object1.a) // somestring
````

Si une clé d'un objet a un espace dans son nom il faut des "" et y accéder avec des []

````javascript
const object2 = {
  a: 'somestring',
  "sp ace": 12
}

console.log(object2["sp ace"]) // 12
````

````javascript
const object3 = {
  a: 'somestring',
  skills: ["html", "css", "javascript", "nodejs", "react", "react native"]
}

console.log(object3.skills[4]) // react
console.log(object3["skills"][4]) // react
````

Copier un objet :

Pour copier un objet simple on peut utiliser Un spread operator ({ ...myObject };
Pour copier en pronfondeur un objet plus complexe on peut depuis mars 2022 utiliser `structuredClone`

````javascript
const myObject = {
    a: "Le Reacteur",
    b: {
        value: "Bootcamp"
    }
}
const clonedObject = structuredClone(myObject);
console.log(clonedObject === myObject)              // false
console.log(clonedObject.b === myObject.b)          // false

const myArray = ["Le Reacteur", { value: "Bootcamp" }];
const clonedArray = structuredClone(myArray);
console.log(clonedArray === myArray)                // false
console.log(clonedArray.at(-1) === myArray.at(-1))  // false
````
