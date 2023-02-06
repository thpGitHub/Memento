# TypeScript

TS PlayGround : <https://www.typescriptlang.org/play>

> Typescript a pour but d'améliorer et de sécuriser la production de code JavaScript. Il s'agit d'un sur-ensemble typé strict de JavaScript.
> TypeScript est un vérificateur de type statique : il détecte les erreurs de code sans executer le programme.  
> Le code TypeScript est transcompilé en JavaScript.

````shell script
    npm i  -g typescript
    tsc nomFichier.ts // cela va créer un fichier .js transpilé en ES5
    tsc --watch nomFichier.ts // surveillera le fichier .ts et mettra à jour le .js
    tsc -w nomFichier.ts
    tsc -w
    tsc --version

    tsc --init // cela va créer un fichier tsconfig.json
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

TypeScript utilisera la valeur comme type. L'inférence de type ne fonctionne que sur l'initialisation.

````typescript
let helloWorld = "Hello World";
//  correspond à :  let helloWorld: string

let hello; //  correspond à :  let hello: any
hello = "hello world" // correspond aussi à :  let hello: any
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
// Création de la variable x avec le tuple [string, number]
let x: [string, number];
x = ["hello", 3]; // ok
x = [3, "hello"]; // error

let p3d: [number, number, number] = [1, 4, 5];

let personne: [string, string, number] = ["Capitaine", "Haddock", 40];
````

````typescript
// création du type Connexion avec le tuple [string, string, string, number, string, string]
type Connexion = [string, string, string, number, string, string]

const google: Connexion = ['Google', 'https', 'google.com', 80, '', '']
const gmail: Connexion = ['Gmail', 'https', 'gmail.com', 80, '', '']

//Création d'un tableau de connection
let connexions: Connexion[] = [google, gmail]
````

Combiner avec un enum pour les valeurs des protocoles

````typescript
enum Protocol {
  HTTP = 'http',
  HTTPS = 'https',
  FTP = 'ftp',
}

type Connexion = [string, Protocol, string, number, string, string]

const google: Connexion = ['Google', Protocol.HTTP, 'google.com', 80, '', '']
const gmail: Connexion = ['Gmail', Protocol.HTTPS, 'gmail.com', 80, '', '']

//Création d'un tableau de connection
let connexions: Connexion[] = [google, gmail]
````

Destructuration du tuple

````typescript
// ici on ne récupère pas la première valeur
let [, gmailProtocol, gmailHostname] = gmail
````

- Enum

> Les Enum sont un moyen de donner des noms plus conviviaux à des ensembles de valeurs numériques.

````typescript
enum Direction {
  Up = 1,
  Down = 2,
  Left = 3,
  Right = 4,
}
let direction: Direction
direction = Direction.Left // 3
````

````typescript
enum HttpStatusCode {
  CONTINUE = 100,
  OK = 200,
  MOVED_PERMANENTLY = 301,
  BAD_REQUEST = 400,
  UNAUTHORIZED = 401,
  FORBIDDEN = 403,
  NOT_FOUND = 404,
  INTERNAL_SERVER_ERROR = 500,
}
let httpResponse: HttpStatusCode = HttpStatusCode.BAD_REQUEST

enum TransfertMessage {
  OK = 'Transfert avec succès',
  KO = 'Erreur durant le transfert',
  RETRY = 'Recommencez le transfert',
}
let message: TransfertMessage = TransfertMessage.OK
````

Incrémentation automatique avec les Enums

````typescript
enum Note {
  STAR1 = 1,
  STAR2,
  STAR3,
  STAR4,
  STAR5,
}
let review: Note
review = Note.STAR3 // 3 car en mettant = 1 à STAR1 typescript incrémente automatiquement la suite
````

On peut aussi mixer les types

````typescript
enum Note {
  STAR1 = 1,
  STAR2,
  STAR3,
  STAR4,
  STAR5,
  NSP = 'Ne se prononce pas'
}
let review: Note
review = Note.NSP //  Ne se pronnoce pas
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

- Discriminated union

````typescript
type JavaDev = {
  langage: 'JAVA'
  framework: string[]
  javaTools: string
}

type JSDev = {
  langage: 'JAVASCRIPT'
  framework: string[]
  jsTools: string
}

type PHPDev = {
  langage: 'PHP'
  framework: string[]
  phpTools: string
}

type Developper = JavaDev | JSDev | PHPDev

function helloDeveloppeur(dev: Developper) {
  // Discriminated
  if (dev.langage === 'JAVA') {
    return `Hello developpeur Java ${dev.javaTools}`
  } else if (dev.langage === 'JAVASCRIPT') {
    return `Hello developpeur JavaScript ${dev.jsTools}`
  } else if (dev.langage === 'PHP') {
    return `Hello developpeur PHP ${dev.phpTools}`
  }
}
const devJava: JavaDev = {
  langage: 'JAVA',
  framework: ['spring', 'spring boot'],
  javaTools: 'JDK',
}
const devJs: JSDev = {
  langage: 'JAVASCRIPT',
  framework: ['React', 'Vue'],
  jsTools: 'Postman',
}
const devPHP: PHPDev = {
  langage: 'PHP',
  framework: ['React', 'Vue'],
  phpTools: 'PHPdebug',
}
````

- Narrowing

````typescript
type JavaDev = {
  langage: 'JAVA'
  framework: string[]
  javaTools: string
}

type JSDev = {
  langage: 'JAVASCRIPT'
  framework: string[]
  jsTools: string
}

type PHPDev = {
  langage: 'PHP'
  framework: string[]
  phpTools: string
}

  type BlockChainDev = {
    langage: string
    framework: string[]
    cryptoBlockChain: string
  }

function helloDeveloppeur(dev: Developper) {
  //Narrowing
  if ('cryptoBlockChain' in dev) {
    return `Hello developpeur Blockchain ${dev.cryptoBlockChain}`
  } else {
    if (dev.langage === 'JAVA') {
      return `Hello developpeur Java ${dev.javaTools}`
    } else if (dev.langage === 'JAVASCRIPT') {
      return `Hello developpeur JavaScript ${dev.jsTools}`
    } else if (dev.langage === 'PHP') {
      return `Hello developpeur PHP ${dev.phpTools}`
    }
  }
}
const devJava: JavaDev = {
  langage: 'JAVA',
  framework: ['spring', 'spring boot'],
  javaTools: 'JDK',
}
const devJs: JSDev = {
  langage: 'JAVASCRIPT',
  framework: ['React', 'Vue'],
  jsTools: 'Postman',
}
const devPHP: PHPDev = {
  langage: 'PHP',
  framework: ['React', 'Vue'],
  phpTools: 'PHPdebug',
}

const ethDev: BlockChainDev = {
  langage: 'JAVASCRIPT',
  framework: ['React', 'Solidity'],
  cryptoBlockChain: 'ETH',
}

type Developper = JavaDev | JSDev | PHPDev | BlockChainDev
````

- exhaustive checks

- types alias

On peut définir nos propres types :

````typescript
type Person = {
  name: string
  age: number
}
let person: Person
let person2: Person
````

```typescript
type number_str = number | string
```

- types litteral

````typescript
type yesNoType = 'yes' | 'no'
let variable: yesNoType

variable = 'yes' // ok
variable = 'blabla' // error
````

````typescript
type Civility = 'Mr' | 'Mme' | 'Mlle'
let civility: Civility
civility = 'Mr'

type maxUploadSize = 2048 | 4096
let uploadSize: maxUploadSize
uploadSize = 2048
````

- types avec intersections (&)

````typescript
type Developper = {
  name: string
}
type FrontEndDev = Developper & {
  frontEndFramework: string
}

type BackEndDev = Developper & {
  backEndFramework: string
}

type FullStackDev = FrontEndDev & BackEndDev

const frontDev: FrontEndDev = {
  name: 'paul',
  frontEndFramework: 'React',
}

const backDev: BackEndDev = {
  name: 'thierry',
  backEndFramework: 'Spring',
}

const fullStackDev: FullStackDev = {
  name: 'steeve',
  backEndFramework: 'Spring',
  frontEndFramework: 'React',
}

// avec les Unions

type StudentDev = FrontEndDev | BackEndDev | FullStackDev
````

- Unknown props

````typescript
interface Club {
  // Unknown props
  [key: string]: object
}
const machesterUnited: Club = {
  cristianoRonaldo: {id: '342', stats: '50buts'},
  paulPogba: {id: '234', stats: '34buts'},
}

const psg: Club = {
  messi: {id: '123', stats: '53buts'},
  mbappe: {id: '545', stats: '76buts'},
}
console.log(Object.keys(machesterUnited).length)
console.log(Object.keys(psg).length)
````

````typescript
type Player = {
  id: string
  stats: string
}
interface Club {
  [key: string]: Player
}
const machesterUnited: Club = {
  cristianoRonaldo: {id: '342', stats: '50buts'},
  paulPogba: {id: '234', stats: '34buts'},
}

const psg: Club = {
  messi: {id: '123', stats: '53buts'},
  mbappe: {id: '545', stats: '76buts'},
}

console.log(machesterUnited.cristianoRonaldo.stats)
console.log(psg.mbappe.stats)
````

- Overloads (Surcharge de fonctions)

````typescript
function printBirthDay(inputDate: Date): string
function printBirthDay(inputDate: string): string
function printBirthDay(inputDate: string | Date): string {
  if (inputDate instanceof Date) {
    return inputDate.toLocaleDateString()
  } else if (typeof inputDate === 'string') {
    return new Date(inputDate).toLocaleDateString()
  } else {
    return 'Non défini'
  }
}

printBirthDay('October 13, 2014')
printBirthDay(new Date(2014, 9, 13))
````

- Overloads avec des paramètres variables

````typescript
type InputDate = string | number | Date

function printBirthDay(year: number, month: number, day: number): string
function printBirthDay(year: number, month: number): string
function printBirthDay(year: number): string
function printBirthDay(inputDate: Date): string
function printBirthDay(inputDate: string): string
function printBirthDay(inputDate: InputDate, m?: number, d?: number): string {
  if (inputDate instanceof Date) {
    return inputDate.toLocaleDateString()
  } else if (typeof inputDate === 'string') {
    return new Date(inputDate).toLocaleDateString()
  } else if (d !== undefined && m !== undefined) {
    return new Date(inputDate, m, d).toLocaleDateString()
  } else if (m !== undefined) {
    return new Date(inputDate, m, 1).toLocaleDateString()
  } else if (typeof inputDate === 'number') {
    return new Date(inputDate, 0, 1).toLocaleDateString()
  } else {
    return 'Non définie'
  }
}

printBirthDay('October 13, 2014')
printBirthDay(new Date(2014, 9, 13))
printBirthDay(2014, 9, 13)
printBirthDay(2014, 9)
printBirthDay(2014)
````

- Overloads avec un type de retour différent

````typescript
type InputDate = string | number | Date

function printBirthDay(year: number, month: number, day: number): string
function printBirthDay(year: number, month: number): string
function printBirthDay(year: number): string
function printBirthDay(inputDate: Date): Date
function printBirthDay(inputDate: string): string
function printBirthDay(inputDate: InputDate, m?: number, d?: number) {
  if (inputDate instanceof Date) {
    return inputDate
  } else if (typeof inputDate === 'string') {
    return new Date(inputDate).toLocaleDateString()
  } else if (d !== undefined && m !== undefined) {
    return new Date(inputDate, m, d).toLocaleDateString()
  } else if (m !== undefined) {
    return new Date(inputDate, m, 1).toLocaleDateString()
  } else if (typeof inputDate === 'number') {
    return new Date(inputDate, 0, 1).toLocaleDateString()
  } else {
    return 'Non définie'
  }
}

printBirthDay('October 13, 2014')
printBirthDay(new Date(2014, 9, 13)).toLocaleDateString()
printBirthDay(2014, 9, 13)
printBirthDay(2014, 9)
printBirthDay(2014)
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

// extends c'est comme les intersections avec les Types
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

## Générics

On utilise les chevrons `<T>` pour définir un génerics afin de construire du code plus flexible.

```typescript
function make<T>(arg: T): T {
  return arg
}
const a = make<string>('hello') //'Hello'
const b = make<[]>([]) //[]
const c = make<number>(2) //2
const d = make<User>(user) //a user
```

Cela fonctionne également avec les `types` et `interfaces`

```typescript
//TYPES
type User<T> = {
  username: string
  personnalData: T
}
const user: User<object> = {
  username: 'Mike',
  personnalData: {settings: {color: 'blue'}},
}
```

- avec les interfaces

```typescript
//INTERFACES
interface User<T> {
  username: string
  personnalData: T
}
const user: User<object> = {
  username: 'Mike',
  personnalData: {settings: {color: 'blue'}},
}
```

Fabriquer une paire

````typescript
function makePair<T>(arg: T): [T, T] {
  return [arg, arg]
}
const p = makePair('mike') //p de type [string, string]
const p2 = makePair(2) //p de type [number, number]
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

---

## React

- props children (children: ReactNode)

````typescript
// App
function App() {
  return (
    <MainContainer>
      <Search></Search>
      <CardContainer>
        <Card />
      </CardContainer>
      <Footer></Footer>
    </MainContainer>
  );
}
````

````typescript
// MainContainer
import React, { ReactNode } from 'react'

type MainContainerProps = {
    children: ReactNode
}

function MainContainer({children}: MainContainerProps) {
  return (
    <main className="container">{children}</main>
  )
}

export default MainContainer
````

---

## `Axios`

Dans TypeScript, vous pouvez spécifier le type de la réponse d'Axios en utilisant le AxiosResponsetype de la axiosbibliothèque. Si la réponse est un tableau d'objets, vous pouvez déclarer le type en tant que tableau d'une interface spécifique qui représente la structure de chaque objet du tableau.

````typescript
import axios, { AxiosResponse } from 'axios';

interface User {
  id: number;
  name: string;
  email: string;
}

async function getUsers(): Promise<AxiosResponse<User[]>> {
  return axios.get<User[]>('https://jsonplaceholder.typicode.com/users');
}

getUsers()
  .then(response => {
    const users = response.data;
    console.log(users);
  })
  .catch(error => {
    console.error(error);
  });
````

Dans cet exemple, l' Userinterface définit la structure de chaque objet du tableau et la getUsersfonction renvoie un Promisede AxiosResponse<User[]>, indiquant que les données de réponse sont un tableau d' Userobjets.

Dans TypeScript, vous pouvez gérer une réponse avec plusieurs interfaces en définissant des interfaces distinctes pour chaque type de réponse, puis en utilisant une union discriminée. Une union discriminée est un type qui représente un ensemble de types, où chaque type a une valeur ou une propriété spécifique qui le distingue des autres.

````typescript
import axios, { AxiosResponse } from 'axios';

interface User {
  id: number;
  name: string;
  email: string;
}

interface Post {
  id: number;
  title: string;
  body: string;
}

type ResponseData = User | Post;

async function getData(type: 'users' | 'posts'): Promise<AxiosResponse<ResponseData[]>> {
  return axios.get<ResponseData[]>(`https://jsonplaceholder.typicode.com/${type}`);
}

getData('users')
  .then(response => {
    const data = response.data;
    data.forEach(item => {
      if ('email' in item) {
        console.log(`User: ${item.name} (${item.email})`);
      } else {
        console.log(`Post: ${item.title}`);
      }
    });
  })
  .catch(error => {
    console.error(error);
  });
````

Dans cet exemple, le ResponseDatatype est une union discriminée qui représente soit un Userobjet, soit un Postobjet. La getDatafonction renvoie un Promisede AxiosResponse<ResponseData[]>, indiquant que les données de réponse sont un tableau d' ResponseDataobjets. Pour déterminer le type de chaque objet du tableau, vous pouvez utiliser l' inopérateur dans une protection de type.

Dans TypeScript, Promise<AxiosResponse<any, any>>est un type qui représente une promesse qui se résout en un objet de type AxiosResponse<any, any>.

Le AxiosResponsetype est défini dans la axiosbibliothèque et il a deux paramètres de type :

data: représente le type des données de réponse
config: représente le type de l'objet de configuration utilisé pour effectuer la requête
Lorsque les deux paramètres de type sont définis sur any, cela signifie que les données de réponse et l'objet de configuration peuvent être de n'importe quel type. Ceci est généralement utilisé lorsque la structure exacte de la réponse et de la configuration est inconnue ou lorsque vous souhaitez permettre une flexibilité maximale.

````typescript
import axios from 'axios';

async function getData(): Promise<axios.AxiosResponse<any, any>> {
  return axios.get('https://jsonplaceholder.typicode.com/posts');
}

getData()
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
````

Dans cet exemple, la getDatafonction renvoie un Promisede AxiosResponse<any, any>, indiquant que les données de réponse et l'objet de configuration peuvent être de n'importe quel type.

## `Event` on `scroll`

````typescript
useEffect(() => {
        const onScroll = (e: Event) => {
            const window = e.currentTarget as Window
            let currentPosition = window.scrollY
            console.log({currentPosition: currentPosition})

            if (currentPosition > 100) {
                setBackgroundStyle('#111')
            } else {
                setBackgroundStyle('transparent')
            }
        }
        window.addEventListener('scroll', e => onScroll(e))

        return () => window.removeEventListener('scroll', onScroll)
    }, [])
````
