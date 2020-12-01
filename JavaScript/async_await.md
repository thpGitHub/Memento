async & await
-


>  Le but des fonctions async/await est de simplifier l'utilisation synchrone des promesses et d'opérer sur des groupes de promesses. De la même façon que les promesses sont semblables à des callbacks structurés, async/await est semblable à la combinaison des générateurs et des promesses.

- ``async`` s'utilise avec une fonction :

````javascript
    let func = async () => {
        
    };
        
    // ou 
    
    async function func1 () {
    
    }
````

Dès que l'on met async devant une fonction cela transforme cette fonction en promesse !
````javascript
    let func = async () => {
        console.log(('ok'));
        return 'test';
    };
    
    func().then(test => console.log(test));
````

- ``await`` s'utilise a l'intérieur d'une fonction async :

````javascript
    let func2 = () => {
      return new Promise((resolve) => {
          setTimeout(()=> {
              resolve('test');
          },500);
      })
    };
    
    let func = async () => {
        console.log(('ok'));
        let text = await func2(); // await attend le résultat d'une promesse !
        return text;
    };
    
    func().then(test => console.log(test));
````