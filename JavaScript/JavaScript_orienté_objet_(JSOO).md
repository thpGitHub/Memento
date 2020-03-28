 # JavaScript

> est conçu autour d'un paradigme simple, basé sur les objets (clé: valeur).
 
#### Les objets et les propriétés
> Une propriété est une variable attachée à un objet. 
 ````javascript
    let monObjet = {};
    // ou
    let monObjet2 = new Objet();

    monObjet.nom = 'Toto';
    // ou
    monObjet['prenom'] = 'Albert'; // comme pour un tableau associatif

    monObjet.age = 25;
    monObjet.adresse; // undefined
````
 > si le nom d'une propriété n'est pas un identifiant valide (contient un tiret, un espace ou débute par un chiffre) elle devra utilisé la notation à crochets.   

>  les valeurs utilisées entre les crochets sont automatiquement converties en chaînes de caractères grâce à la méthode ``toString()`` sauf si ces valeurs sont des symboles.

#### il existe trois méthodes natives pour lister/parcourir les propriétés d'un objet :

   - Les boucles ``for...in`` qui permettent de parcourir l'ensemble des propriétés énumérables d'un objet et de sa chaîne de prototypes.
   - ``Object.keys(o)`` qui permet de renvoyer un tableau contenant les noms (clés ou keys) des propriétés propres (celles qui ne sont pas héritées via la chaîne de prototypes) d'un objet o pour les propriétés énumérables.
   - ``Object.getOwnPropertyNames(o)`` qui permet de renvoyer un tableau contenant les noms des propriétés propres (énumérables ou non) d'un objet o.   
    
#### Un objet peut être crée de différentes manières :
   - avec une fonction constructeur
   - avec des initialisateurs d'objets :
        - objet literal
        - new Object()
        - Object.create() 
 
   
 ##### Création d'un objet avec une fonction constructeur : 
 
>Le constructeur est l'équivalent JavaScript d'une classe.

>Il possède l'ensemble des fonctionnalités d'une fonction, cependant il ne renvoie rien et ne crée pas d'objet explicitement.

```javascript
function Personne(nom) {
        this.nom = nom;
        // salutation est une méthode de Personne
        this.salutation = function() {
            alert('Bonjour ! Je m\'appelle ' + this.nom + '.');
        };
    }
```
>Par convention : Les fonctions de type constructeur commencent généralement par une majuscule
```javascript
let personne1 = new Personne('Bob'),  //l'objet personne1 est une instance de Personne
    personne2 = new Personne('Sarah');

personne1.nom
personne1.salutation()
personne2.nom
personne2.salutation()
```
```javascript
function Personne(prenom, nom, age, genre, interets) {
  this.nom = {
    prenom,
    nom
  };
  this.age = age;
  this.genre = genre;
  this.interets = interets;
  this.bio = function() {
    alert(this.nom.prenom + ' ' + this.nom.nom + ' a ' + this.age + ' ans. Il aime ' + this.interets[0] + ' et ' + this.interets[1] + '.');
  };
  this.salutation = function() {
    alert('Bonjour ! Je m\'appelle ' + this.nom.prenom + '.');
  };
}

let personne1 = new Personne('Bob', 'Smith', 32, 'homme', ['musique', 'ski']);
```
 
 ##### Créer un objet literal
 ````javascript
    let obj = { propriete_1:   valeur_1,   // propriete_# peut être un identifiant
                 2:             valeur_2,   // ou un nombre
                 // ...,
                 "propriete n": valeur_n }; // ou une chaîne
````
 
 
 
 ##### Utiliser le constructeur ``Object()``
 
 ##### Utiliser une fonction constructeur personnalisée
 
 ##### Utiliser la méthode ``create()``
 
 
 
 
 
 # Le Constructeur

##### 2eme technique : Le constructeur Object()
```javascript
var personne1 = new Object();

personne1.nom = 'Chris';
personne1['age'] = 38;
personne1.salutation = function() {
  alert('Bonjour ! Je m\'appelle ' + this.nom + '.');
};
```
```javascript
var personne1 = new Object({
  nom: 'Chris',
  age: 38,
  salutation: function() {
    alert('Bonjour ! Je m\'appelle ' + this.nom + '.');
  }
});
```
##### 3eme technique : méthode create()
```javascript
var personne2 = Object.create(personne1);
```
>personne2 a été créée à partir de personne1 — et elle possède les mêmes propriétés. 
````javascript
const person = {
  isHuman: false,
  printIntroduction: function () {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};

const me = Object.create(person);

me.name = "Matthew"; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // inherited properties can be overwritten

me.printIntroduction();
// expected output: "My name is Matthew. Am I human? true"

````
