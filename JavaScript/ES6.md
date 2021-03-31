# ES6

## Destructuring

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
