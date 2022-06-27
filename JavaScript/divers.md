# Divers JS

```javascript
    const children = "Bienvenue sur mon site";
    const className = "rootContainer";
    const props = { children, className };
    
    console.log(props);
    // {children: 'Bienvenue sur mon site', className: 'rootContainer'}
    
    const props2 = [children, className];
    console.log(props2);
    // ['Bienvenue sur mon site', 'rootContainer']
```

---

```javascript
const [type] = useState(['movie', 'tv'][getRandomIntInclusive(0,1)])

// export function getRandomIntInclusive(min, max) {
//   min = Math.ceil(min)
//   max = Math.floor(max)
//   return Math.floor(Math.random() * (max - min + 1)) + min
// }
```
