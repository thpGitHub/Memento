# TypeScript

> Typescript a pour but d'améliorer et de sécuriser la production de code JavaScript. Il s'agit d'un sur-ensemble typé strict de JavaScript.
> TypeScript est un vérificateur de type statique : il détecte les erreurs de code sans executer le programme.  
> Le code TypeScript est transcompilé en JavaScript.

````shell script
    $ npm i  -g typescript
    $ tsc nomFichier.tsc // cela va créer un fichier .js transpilé en ES5
    $ tsc --watch nomFichier.tsc // surveillera le fichier .ts et mettra à jour le .js
    $ tsc --version
````

## Type par inférence

TypeScript utilisera la valeur comme type.

````typescript
  let helloWorld = "Hello World";
//  correspond à :  let helloWorld: string

````

## Le type Tuple

````typescript
    const addresse: [string, number] = ['rue de l\'espérance', 17];
````

>>TypeScript considère chaque fichier .ts soit :

- comme un module
- soit comme un script

> Dans le cas d'un module, le code à l'intérieur du fichier est isolé du reste du code contenu dans les autres fichiers (rappelez-vous Webpack). Chaque module peut importer d'autres modules, appelées dépendances du modules, et aussi exporter des données pour les rendre visible par d'autres modules.
  
  Si TypeScript ne rencontre ni "import" ni "export" dans un fichier, celui-ci sera considéré comme un scrip JS. Dans ce cas tout le code contenu dans le script sera considéré comme global et partagé avec les autres scripts.
  
> DONC, pour résoudre le problème, il suffit de dire à TypeScript que notre fichier .ts est un module.
  C'est accompli en ajoutant un :
  
  Attention à voir pour l'export à la fin
  export {};
  
  à la fin du fichier

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
