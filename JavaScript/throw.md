# throw

En JavaScript, `throw` est un mot clé utilisé pour créer et lever une exception. Une exception est une erreur d'exécution qui se produit lors de l'exécution d'un programme.

Lorsqu'une `throw` instruction est rencontrée dans un programme JavaScript, elle provoque la levée d'une exception, qui arrête le déroulement normal du programme et transfère le contrôle à un gestionnaire d'exceptions. Le gestionnaire d'exceptions peut être défini à l'aide d'un `try...catch` bloc, ce qui vous permet d'attraper et de gérer l'exception levée.

Voici un exemple de la façon dont throw peut être utilisé en JavaScript :

````javascript
function divide(a, b) {
  if (b === 0) {
    throw "Cannot divide by zero";
  }
  return a / b;
}

try {
  let result = divide(10, 0);
  console.log(result);
} catch (e) {
  console.error("An error occurred: " + e);
}
````

Dans cet exemple, la divide fonction vérifie si le b paramètre est égal à zéro, et si c'est le cas, elle lève une exception avec le message "Impossible de diviser par zéro".

Le `try...catch` bloc est utilisé pour intercepter l'exception levée et la gérer. Dans ce cas, le bloc `catch` consigne un message d'erreur dans la console.

Dans l'ensemble, `throw` est un outil puissant en JavaScript pour gérer les erreurs et prévenir les comportements inattendus dans votre code.

## différence entre `throw "coucou"` and `throw new Error("coucou")` ?

En JavaScript, `throw "coucou"` et `throw new Error("coucou")` sont tous deux utilisés pour générer une erreur, mais ils présentent des différences importantes.

`throw "coucou"` est utilisé pour lancer une chaîne en tant qu'erreur. C'est ce qu'on appelle un "lancement de chaîne" et n'est généralement pas recommandé car il ne crée pas d' Error objet avec une trace de pile, ce qui rend plus difficile le débogage des erreurs. Lorsqu'une chaîne est émise, elle est capturée en tant qu'objet d'erreur avec la valeur de chaîne affectée à sa propriété message.

D'autre part, `throw new Error("coucou")` crée un nouvel Error objet avec un message "coucou" et le lance comme une erreur. Il s'agit de la méthode préférée pour générer des erreurs en JavaScript, car elle crée un Error objet avec une trace de pile qui fournit plus d'informations sur l'endroit où l'erreur s'est produite dans le programme. L' Error objet peut également contenir des informations supplémentaires, telles qu'un code d'erreur ou d'autres métadonnées, pour aider à identifier et gérer les erreurs plus efficacement.

En résumé, `throw new Error("coucou")` est généralement préféré à `throw "coucou"` car il crée un Error objet avec plus d'informations qui peuvent être utilisées pour identifier et gérer les erreurs plus efficacement.

```javascript
function divide(a, b) {
  if (b === 0) {
    throw new Error("Cannot divide by zero");
  }
  return a / b;
}

try {
  let result = divide(10, 0);
  console.log(result);
} catch (e) {
  console.error("An error occurred: " + e.message);
}
````

## les différents gestionnaires d'exceptions ?

En JavaScript, il existe plusieurs types de gestionnaires d'exceptions différents qui peuvent être utilisés pour intercepter et gérer les erreurs :

- `try-catch` block : il s'agit du gestionnaire d'exceptions le plus courant en JavaScript. Il se compose d'un `try` bloc qui contient du code susceptible de générer une erreur et d'un `catch` bloc qui intercepte l'erreur et la gère. Le `catch` bloc peut être utilisé pour afficher un message d'erreur, enregistrer l'erreur ou effectuer toute autre gestion d'erreur appropriée.

- `try-finally` bloc : Ceci est similaire à un `try-catch` bloc, mais au lieu d'un `catch` bloc, il a un `finally` bloc qui s'exécute toujours, qu'une erreur soit levée ou non. Le `finally` bloc peut être utilisé pour effectuer toutes les actions de nettoyage nécessaires, telles que la fermeture d'un fichier ou la libération d'une ressource.

- `catch` déclaration : Ceci est similaire à un `try-catch` bloc, mais il est utilisé pour intercepter des types d'erreurs spécifiques. La `catch` instruction spécifie le type d'erreur qu'elle peut gérer et le `catch` bloc n'est exécuté que si ce type d'erreur est généré. Cela peut être utile pour gérer différemment différents types d'erreurs, comme une erreur réseau par rapport à une erreur de validation.

- `window.onerror` : Il s'agit d'un gestionnaire d'erreurs global qui peut être utilisé pour intercepter les exceptions non gérées. Il est déclenché lorsqu'une erreur se produit mais n'est intercepté par aucun `try-catch` bloc. Il peut être utilisé pour afficher un message d'erreur personnalisé ou consigner l'erreur.

- `Promise.catch` : Il s'agit d'une méthode de gestion des erreurs qui peut être utilisée avec des promesses. Il est appelé lorsqu'une promesse est rejetée et permet de gérer les erreurs de manière plus asynchrone.

En utilisant ces différents gestionnaires d'exceptions, les développeurs peuvent créer des applications plus robustes et plus fiables qui gèrent les erreurs plus efficacement.
