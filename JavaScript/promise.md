Les promesses en JavaScript ( promises )
-

> JavaScript a un thread unique, ce qui signifie que deux scripts ne peuvent pas s'exécuter en même temps, 
> ils doivent s'exécuter l'un derrière l'autre.   
> Une promesse est un objet qui représente la résolution (resolve) ou l'échec (reject) d'une opération asynchrone.

````javascript
    // Création d'une promesse : 
    const promise1 = new Promise(function (resolve, reject) {
        resolve(44);
        reject('ko');  
    });

    // Capter l'état de la promesse :
    promise1
        .then(function (value_Resolve_Promise1) { // la methode .then fait référence à resolve
            console.log(valueOfResolveOfPromise1); // output : 44
        })
        .catch(function(value_reject_promise1) { // la methode .catch fait référence à reject
            console.error(value_reject_promise1);   // output : ko
        })    


````
Une promesse a trois états :
- Pending (En attente) : c'est l'état initial
- Fulfilled (accompli) : l'opération est terminée avec succès
- Rejected             : l'opération a échouée

````javascript
    const promise = new Promise(function(resolve, reject) {
      // do thing, then…
    
      if (/* ....  */) {
        resolve("See, it worked!");
      }
      else {
        reject(Error("It broke"));
      }
    });
````

### Chainer les promesses avec ```then```
> Il existe 4 méthodes statiques dans la classe Promise:

- Promise.resolve
- Promise.reject
- Promise.all
- Promise.race


> Pour synchroniser les uns derrière les autres plusieurs appels asynchrones, on peut chainer les promesses
> afin de pouvoir utiliser la valeur de la première promesse dans les appels suivants. 

````javascript
    var add = function(x, y) {
      return new Promise((resolve,reject) => {
        var sum = x + y;
        if (sum) {
          resolve(sum);
        }
        else {
          reject(Error("Could not add the two values!"));
        }
      });
    };
    
    var subtract = function(x, y) {
      return new Promise((resolve, reject) => {
        var sum = x - y;
        if (sum) {
          resolve(sum);
        }
        else {
          reject(Error("Could not subtract the two values!"));
        }
      });
    };
    
    // Starting promise chain
    add(2,2)
      .then((added) => {
        // added = 4
        return subtract(added, 3);
      })
      .then((subtracted) => {
        // subtracted = 1
        return add(subtracted, 5);
      })
      .then((added) => {
        // added = 6
        return added * 2;    
      })
      .then((result) => {
        // result = 12
        console.log("My result is ", result);
      })
      .catch((err) => {
        // If any part of the chain is rejected, print the error message.
        console.log(err);
      });
````

---
### Générateurs de fonctions (A voir)
---

### Promise.all (itérable) est très utile pour les demandes multiples à différentes sources
> La méthode Promise.all (itérable) renvoie une seule promesse qui se résout lorsque toutes les promesses de l'argument itérable ont été résolues ou lorsque l'argument itérable ne contient aucune promesse. Il rejette avec la raison de la première promesse qui rejette.

````javascript
    var promise1 = Promise.resolve(catSource);
    var promise2 = Promise.resolve(dogSource);
    var promise3 = Promise.resolve(cowSource);
    
    Promise.all([promise1, promise2, promise3]).then(function(values) {
      console.log(values);
    });
    // expected output: Array ["catData", "dogData", "cowData"]

````