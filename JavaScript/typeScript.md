# TypeScript

> Typescript a pour but d'améliorer et de sécuriser la production de code JavaScript. Il s'agit d'un sur-ensemble typé strict de JavaScript.
> TypeScript est un vérificateur de type statique : il détecte les erreurs de code sans executer le programme.  
> Le code TypeScript est transcompilé en JavaScript.

````shell script
    npm i  -g typescript
    tsc nomFichier.ts // cela va créer un fichier .js transpilé en ES5
    tsc --watch nomFichier.ts // surveillera le fichier .ts et mettra à jour le .js
    tsc -w nomFichier.ts
    tsc --version
````

>TypeScript considère chaque fichier .ts soit :

- comme un module
- soit comme un script

> Dans le cas d'un module, le code à l'intérieur du fichier est isolé du reste du code contenu dans les autres fichiers (rappelez-vous Webpack). Chaque module peut importer d'autres modules, appelées dépendances du modules, et aussi exporter des données pour les rendre visible par d'autres modules.
  
  Si TypeScript ne rencontre ni "import" ni "export" dans un fichier, celui-ci sera considéré comme un scrip JS. Dans ce cas tout le code contenu dans le script sera considéré comme global et partagé avec les autres scripts.
  
> DONC, pour résoudre le problème, il suffit de dire à TypeScript que notre fichier .ts est un module.
  C'est accompli en ajoutant un :
  
  Attention à voir pour l'export à la fin
  `export {};`
  
  à la fin du fichier

## Type par inférence

TypeScript utilisera la valeur comme type.

````typescript
  let helloWorld = "Hello World";
//  correspond à :  let helloWorld: string

````

---

## Basic Types

- Boolean

````typescript
let isDone: boolean = false;
````

- Number

````typescript
let nb: number = 6;
````

- String

````typescript
let color: string = "red"; 
color = 'blue';
let fullName: string = `Spongebob`; // template strings
````

- Array

````typescript
let tab: number[] = [1, 2, 3];
let tab: Array<number> = [1, 2, 3]; // generic array type
````

- Tuple

````typescript
let x: [string, number];
x = ["hello", 3]; // ok
x = [3, "hello"]; // error

let p3d: [number, number, number] = [1, 4, 5];

let personne: [string, string, number] = ["Capitaine", "Haddock", 40];
````

- Enum

> Les Enum sont un moyen de donner des noms plus conviviaux à des ensembles de valeurs numériques.

````typescript
enum Color {
  Red,
  Green,
  Blue,
}
let c: Color = Color.Green;
console.log(c); // 1

enum Color {
  Red = 1,
  Green,
  Blue,
}
let c: Color = Color.Green;
console.log(c); // 2

enum Color {
  Red = 1,
  Green,
  Blue,
}
let colorName: string = Color[2];
console.log(colorName); // 'Green'
````

- Unknown

> Si nous ne connaissons pas le type de la variables lorsque nous écrivons le code. Le type unknown indique au compilateur et aux futurs lecteurs que cette variable pourrait être n'importe quoi.

````typescript
let notSure: unknown = 4;
notSure = "maybe a string instead";
notSure = false;
````

- Any

>désactive la vérification de type.

````typescript
let quelqueChoseDeFlou: any = 4;
quelqueChoseDeFlou = true;
quelqueChoseDeFlou = document.body;

const tableauMixte: any[] = [true, "chaussette", 3.14];
````

- Void

> void c'est un peu le contraire de any

````typescript
// le type de retour de la fonction ne retourne pas de valeur:

function warnUser(): void {
  console.log("This is my warning message");
````

- Null and Undefined

> Par défaut nullet undefinedsont des sous-types de tous les autres types. Cela signifie que vous pouvez attribuer null et undefined à number par exemple.

````typescript
let u: undefined = undefined;
let n: null = null;
````

- Never

- Object

````typescript
let fiche: { essence: string; taille: number } = {
  essence: "chêne feuillu",
  taille: 12,
};

let fiche2: {
  essence: string;
  taille: number;
  caracteristiques: string[];
  faune: { oiseaux: string[] };
  pousser: () => number;
  ensoleillement: (x: number) => boolean; // l'argument peut prendre n'importe quel nom ici
  scier: () => void;
} = {
  essence: "chêne feuillu",
  taille: 12,
  caracteristiques: ["vert", "Europe", "sempervirent"],
  faune: {
    oiseaux: ["pie", "rouge-gorge", "merle"],
  },
  pousser: function () {
    this.taille++;
    return this.taille;
  },
  ensoleillement: function (intensite) {
    return true;
  },
  scier: function () {
    console.log("Zzzzzz Crrrrac BOOOOM");
  },
};

````

- union types

````typescript
let conditionOuNombre = true; // TypeScript a inféré (deviné) le type boolean
conditionOuNombre = 5; // error

let nombreOuCondition: number | boolean | string = true; // TypeScript a inféré (deviné) le type boolean
nombreOuCondition = 5;
nombreOuCondition = "un texte";
````

---

## Interface

````typescript
// déclaration de l'interface
interface Identite {
  nom: string;
  pays: string;
  age: number;
}

interface IdentiteAvecNationaliteX extends Identite {
  nationalité: string;
  x?: any;
}
interface IdentiteAvecNationaliteY extends Identite {
  nationalité: string;
  y?: any;
}
interface IdentiteAvecNationaliteZ extends Identite {
  nationalité: string;
  z?: any;
}

const voyageur1: IdentiteAvecNationaliteY = {
  nom: "Pierre",
  pays: "France",
  age: 21,
  nationalité: "française",
  y: true,
};

const voyageur2: IdentiteAvecNationaliteX = {
  nom: "Paul",
  pays: "France",
  age: 29,
  nationalité: "française",
};

// Un objet de type IdentiteAvecNationaliteY ou IdentiteAvecNationaliteX est aussi de type Identite (les types dérivés sont compatibles avec les types de base)
var voyageurs: Identite[] = [voyageur1, voyageur2];
````

---

## Function

````typescript
// named function with return type number
function add(x: number, y: number): number {
  return x + y;
}

// anonymous function with return type number
let myAdd = function (x: number, y: number): number {
  return x + y;
};

//***************************************************
const addition = (a: number, b: number): number => {
  return a + b;
};

addition(5, 6);
addition(10); // problème, argument b manquant
addition(10, "20"); // string incompatible avec number

// ici, b est optionnel car possède une valeur par défaut
const multiplication = (a: number, b: number = 1): number => {
  return a * b;
};

multiplication(5); // 5

// Paramètre optionnel
const test = (a: number, b?: number): any => {
  if (b !== undefined) {
    return a + b;
  } else {
    return false;
  }
};
const test1: (x: number, y?: number) => any = (a, b) => {
  if (b !== undefined) {
    return a + b;
  } else {
    return false;
  }
};

const alerterEncore = (message: string): void => {
  window.alert(message);
  return undefined;
};

````

---

## Class

````typescript
class Animal {
  nom: string;

  constructor(nomDeLAnimal: string) {
    this.nom = nomDeLAnimal;
  }

  bouger(distanceEnMetres: number = 0): void {
    console.log(`L'animal bouge de ${distanceEnMetres}m.`);
  }
}

const animalInconnu = new Animal("le dodo");
animalInconnu.bouger(4);

class Chien extends Animal {
  fourrure: boolean;

  constructor(nomDunChien: string) {
    super(nomDunChien);
    this.fourrure = true;
  }

  aboiement(): void {
    console.log("Ouaf ! Ouaf !");
  }
}

const medor = new Chien("Médor");
medor.bouger(10);
medor.aboiement();

/*
  modificateurs de portée :
  - public
  - private
  - protected

  S'appliquent sur :
  - les propriétés de la classe
  - les méthodes de la classe
*/

class Personne {
  protected nom: string; // accessible depuis les classes dérivées
  constructor(nom: string) {
    this.nom = nom;
  }
}

class Employe extends Personne {
  private domaine: string; // accessible uniquement depuis Employe

  constructor(nom: string, domaine: string) {
    super(nom);
    this.domaine = domaine;
  }

  public sePresenter() {
    // ici nom est accessible car déclaré avec protected
    return `Bonjour, je m'appelle ${this.nom} et je travaille dans ${this.domaine}.`;
  }
}

class EmployeNouveau extends Employe {
  public direNom() {
    console.log(this.nom); // nom est accessible dans n'importe quelle classe dérivée (quel que soit le niveau d'héritage)
  }
}

// instance de Employe
const employeDuMois = new Employe("Jean-Luc", "comptabilité");

employeDuMois.sePresenter(); // public par défaut -> OK
employeDuMois.domaine; // private, n'est accessible qu'à l'intérieur de la classe
employeDuMois.nom; // protected, n'est accessible que dans sa classe et les classes dérivées

````
