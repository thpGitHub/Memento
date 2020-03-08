
# Le Constructeur

>Le constructeur est l'équivalent JavaScript d'une classe.
>
>Il possède l'ensemble des fonctionnalités d'une fonction, cependant il ne renvoie rien et ne crée pas d'objet explicitement.

##### 1ere technique : le constructeur (function)
```javascript
function Personne(nom) {
        this.nom = nom;
        this.salutation = function() {
            alert('Bonjour ! Je m\'appelle ' + this.nom + '.');
        };
    }
```
>Par convention : Les fonctions de type constructeur commencent généralement par une majuscule
```javascript
let personne1 = new Personne('Bob'),
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
