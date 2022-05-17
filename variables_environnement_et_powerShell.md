# Variables environnement et PowerShell

## Windows

### PowerShell vs cmd

*cmd* : L’invite de commande Windows est un interpréteur de commandes basé sur le système d’exploitation MS-DOS des années 1980 qui permet à un utilisateur d’interagir directement avec le système d’exploitation.

*PowerShell* : Windows PowerShell est un interpréteur de commandes et un langage de script conçus pour les tâches d’administration système. Il a été construit sur le framework .NET, une plate-forme de programmation logicielle développée par Microsoft en 2002.

*PowerShell* et *cmd* sont très puissants. PowerShell est un nouveau produit de Microsoft, il offre de nombreuses fonctionnalités comparé à CMD. Peut faire beaucoup d’automatisation. Il aide l’administrateur système à automatiser très facilement la tâche à l’aide de PowerShell, fournit un grand nombre de commandes par rapport à CMD et offre davantage de fonctionnalités.

### Variables environnement

En informatique, les variables d’environnement sont des variables dynamiques utilisées par les différents processus d’un système d’exploitation (Windows, Unix...). Elles servent à communiquer des informations entre les programmes qui ne se trouvent pas sur la même ligne hiérarchique, et qui ont donc besoin d'une convention pour se communiquer mutuellement leurs choix.

```shell script
# permet dans cmd de lister les variables d'environnement
SET
```

Sous Windows, les variables d'environnement sont entourées du caractère « % ».

```shell script
echo %NOM_DE_LA_VARIABLE%

echo %APPDATA% # C:\Users\thier\AppData\Roaming
set NomVariable # afficher une variable
set NomVariable=valeur # créer une variable
set NomVariable= # supprimer une variable
set couleur=noir^&blanc # utiliser des caractères spéciaux (<, >, |, & ou ^)
# ou
set varname="new&name"
```
