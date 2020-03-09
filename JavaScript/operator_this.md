operator this
===
> La valeur de this représente le contexte dans lequel le code courant est exécuté.
> Dans la plupart des cas, la valeur de this sera déterminée à partir de la façon dont une fonction est appelée.

- Dans le contexte global
- Dans le contexte d'une fonction
    - Appel simple
        - call() 
        - apply()
        - bind()
        - arrows functions
    - En tant que méthode d'un objet
    - En tant que constructeur
    - En tant que gestionnaire d'événement DOM
    - En tant que gestionnaire d'événements in-line
 ---  
### Dans le contexte global
> Dans un contexte en dehors de toute fonction, this fait référence à l'objet global
````javascript
    console.log(this); // window {} (window ici est l'objet global) 

    this.a = 'Hey!'; 
    console.log(window.a); // Hey!
    console.log(a);        // Hey!
````
---

### Dans le contexte d'une fonction
> La valeur de this dépendra de la façon dont la fonction a été appelée.

##### Valeur de this avec un appel simple
