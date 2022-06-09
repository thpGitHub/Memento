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

````javascript
const [status, setStatus] = useState({
    loading: null,
    done: null,
    fail: null,
  })

setStatus({...status, loading: 'Loading'})  
setStatus({...status, loading: null, fail: null})
````

```javascript
const [email, setEmail] = useState({value: '', empty: false})

setEmail({...email, ...{empty: true}})
// fait la même chose !!
setEmail({...email, empty: true})
```

```javascript
const [form, setForm] = useState({
    email: {value: '', empty: false, valid: false},
    psw: {value: '', empty: false},
})
 
const handlerInputChange = e => {
    setForm(...formTemp, ...{email: {value: '', empty: true, valid: true}},)
}
// ou 
const handleSubmit = e => {
    e.preventDefault()

    let formTemp = {...form}

    if (form.email.value === '') {
      formTemp = {
        ...formTemp,
        ...{email: {value: '', empty: true, valid: true}},
      }
    }
```

Autre exemple 

````javascript
const [dataForm, setDataForm] = useState({
        username: '',
        email: '',
        password: '',
        newsletter: false,
    })

    const handleInputChange = e => {
        e.target.name === 'newsletter'
            ? setDataForm({...dataForm, [e.target.name]: e.target.checked})
            : setDataForm({...dataForm, [e.target.name]: e.target.value})
    }
````

## Copie en surface (shallow copy) vs copie en pronfondeur (deep copy) d'un objet ou d'un array

Le `spread operator` permet de faire une copie en surface, cad une copie de premier niveau :
copie les primitives telles que les nombres, les valeurs booléennes, les chaînes et les arrays !!!, mais ne copie aucune référence aux objets !

````javascript
const myObject = {
    a: "un",
    b: {
        value: "Deux"
    }
}
constnewObject = { ...myObject };
console.log(newObject === myObject)              // false
console.log(newObject.b === myObject.b)          // true 
````

Ici le `spread operator` fait une copie de `a` mais pas de `b`. `b` est une copie par référence de `myObjet` pcq b est un objet !

````javascript
const obj = {
    a: 1,
    b: [2,3]
}

const newObj = {...obj}

obj.b = [99,99]

console.log(obj)     // { a: 1, b: [ 99, 99 ] }
console.log(newObj)  // { a: 1, b: [ 2, 3 ] }
````

Ici `b` est bien copie car c'est un `array` !!

### Pour copier en profondeur

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

## Optional chaining

L'opérateur `?.` permet de ne pas causer d'erreur si une référence est `null` ou `undefined`, l"expression s'arrête et retourne la valeur `undefined`. Quand il est utilisé avec des appels de fonctions, il retourne undefined si la fonction donnée n'existe pas.

L'`Optional chaining` ne peut pas être utilisé sur un objet initialement inexistant.

```javascript
const adventurer = {
  name: 'Alice',
  cat: {
    name: 'Dinah'
  }
};

const dogName = adventurer.dog?.name;
console.log(dogName);
// expected output: undefined

console.log(adventurer.someNonExistentMethod?.());
// expected output: undefined
```

Combinaison avec `Nullish coalescing operator`

```javascript
let customer = {
  name: "Carl",
  details: { age: 82 }
};
const customerCity = customer?.city ?? "Unknown city";
console.log(customerCity); // Unknown city
```

```javascript
<h1> {movie?.title ?? '...'} </h1>
```

## Nullish coalescing operator

L'opérateur `??` est un opérateur logique qui renvoie la valeur de droite si la valeur de gauche est `null` ou `undefined`

La difference avec l'oprateur `||`, est que `||` renvoie la valeur de droite si la valeur de gauche est fausse, pas seulement `null` ou `undefined` mais aussi par exemple `''` ou `0`

```javascript
const foo = null ?? 'default string';
console.log(foo);
// expected output: "default string"

const baz = 0 ?? 42;
console.log(baz);
// expected output: 0
```
