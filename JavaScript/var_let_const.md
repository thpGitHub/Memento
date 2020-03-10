
> Au niveau le plus haut (la portée globale), let crée une variable globale alors que var ajoute une propriété à l'objet global :

````javascript
    var x = 'global';
    let y = 'global2';
    
    console.log(this.x); // "global"
    console.log(this.y); // undefined
    console.log(y);      // "global2"
````