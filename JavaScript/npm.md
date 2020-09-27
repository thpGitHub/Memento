npm
-
> Désinstaller un package
````shell script
    $ npm uninstall <package_name>
    $ npm uninstall --save <package_name>
    // si le package a été installé en tant que ``devDependency``
    $ npm uninstall --save-dev <package_name>
````
> Pour vérifier si le package a bien été désinstallé il faut check le dossier ``node_modules``
> ainsi que le fichier ``package.json``

- le module ``dotenv``
> ce module charge les variables d'environnement à partir d'un fichier ``.env`` 
 A voir : https://github.com/motdotla/dotenv
