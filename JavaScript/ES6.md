# ES6

## Destructuring

```javascript
const personne = {nom: 'john', prenom: 'doe', age: 25, ville: 'paris'}

function sayHello(personne) {
  const nom = personne.nom
  const prenom = personne.prenom
  const age = personne.age
  const ville = personne.ville
  console.log(`Bonjour ${nom} ${prenom} tu as ${age} à ${ville}`)
}
// ↓ identique à ↓

//même fonction mais avec la destructuration
function sayHello(personne) {
  const {nom, prenom, age, ville} = personne
  console.log(`Bonjour ${nom} ${prenom} tu as ${age} à ${ville}`)
}

//on peut aussi faire la destructuration en parametre
function sayHello({nom, prenom, age, ville}) {
  console.log(`Bonjour ${nom} ${prenom} tu as ${age} à ${ville}`)
}
sayHello(personne)
```

````javascript
for(let { name, age } of users) {
    console.log(`${name} is ${age} years old!`);
}
`````

````javascript
const getUser = () => {
    return{ 
        'name': 'Alex',
        'address': '15th Park Avenue',
        'age': 43
    }
}

const { name, age } = getUser();

console.log(name, age); // Alex 43
````

````javascript
function logDetails({name, age}) {
    console.log(`${name} is ${age} year(s) old!`)
}
````

````javascript
let emojis = ['🔥', '⏲️', '🏆', '🍉'];

let [fire, clock, , watermelon] = emojis;

console.log(fire, clock, watermelon); // 🔥 ⏲️ 🍉
````

## Spread Operator and Rest Parameter
