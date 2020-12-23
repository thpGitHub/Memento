TypeScript
-
> Typescript a pour but d'améliorer et de sécuriser la production de code JavaScript. Il s'agit d'un sur-ensemble typé strict de JavaScript.
> TypeScript est un vérificateur de type statique : il détecte les erreurs de code sans executer le programme.  
> Le code TypeScript est transcompilé en JavaScript.

````shell script
    $ npm i  -g typescript
    $ tsc nomFichier.tsc // cela va créer un fichier .js transpilé en ES5
    $ tsc --watch nomFichier.tsc // surveillera le fichier .ts et mettra à jour le .js
    $ tsc --version
````

### Type par inférence : 
TypeScript utilisera la valeur comme type.
````typescript
  let helloWorld = "Hello World";
//  correspond à :  let helloWorld: string

````

### Le type Tuple

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