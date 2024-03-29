# form

> Tous les attributs de la balises ``<form>`` sont optionnels.

## Il existe différente techniques pour envoyer un formulaire

- Attribut type="submit"

````html
    <form action="/login.pug" method="post">
    
        <input type="submit" name="submit" value="Login">
        <!-- ou -->
        <button type="submit">Login</button>

    </form>
````

> La différence entre un ``<input type="submit"`` et ``<button type="submit"`` est que input ne permet d'utiliser que du text
> alors que l'élément button permet d'utiliser n'importe quel contenu HTML, autorisant ainsi des textes de bouton plus complexes et créatif.

- Attribut type="button" et onclick

````html
    <form id="myForm" action="/login.pug" method="post">
        
        <input type="button" onclick="myFunction()" value="Login">
        <!-- ou -->
        <button type="button" onclick="myFunction()">Login</button>
    
    </form>
    <script>
         function myFunction() {
            document.getElementById("myForm").submit();         
}   
    </script>
````

- Attribut onSubmit

> Si la méthode checkMyForm() return false le formulaire ne sera pas envoyé !

````html
    <form action="/login.pug" method="post" onSubmit="return checkMyForm()">
            
           <input type="submit" name="submit" value="Login">
           <!-- ou -->
           <button type="submit">Login</button>
        
        </form>
        <script>
             function checkMyForm() {
                if (toto) {
                    return true;            
                }
                else {
                    return false;                
                }               
    }
    // A voir la version simplifié
    //function checkMyForm() {
    //                    return !!toto;               
    //    }      
        </script>
````

- La méthode  ``HTMLFormElement.submit()``  soumet un ``<form>`` donné.

````javascript
    document.forms["myform"].submit();
````

> La propriété ``forms`` de ``document`` retourne une collection (HTMLCollection) des éléments ``<form>`` présent dans le document actuel

````javascript
    document.querySelector('form').submit();
````

- La méthode  ``HTMLFormElement.reset()``  restaure les valeurs par défaut des éléments du formulaire.

````javascript
    document.querySelector('form').reset();
````

### ATTENTION à l'erreur suivante

>``Uncaught TypeError: document.querySelector(...).submit is not a function at XMLHttpRequest.request.onload``

Pour résoudre ce problème il faut modifier le name du button ou de l'input:

````html

    <input type="submit"   name="submit" value="Login">

    <!-- en -->

    <input type="submit"   name="btn_submit" value="Login">

````

Pourquoi cette erreur : car lorsque l'on name le button ou l'input en submit on remplace la fonction submit() du formulaire.
Cela peut aussi se produire avec un id="submit".

---

### FormData

L'interface FormData permet de construire facilement un ensemble de paires clé/valeur représentant les champs du formulaire et leurs valeurs, qui peuvent ensuite être facilement envoyées.

L'objet FormData possède un ensemble de méthodes afin de pouvoir le manipuler !

```javascript
// Le constructeur FormData() crée un nouvel objet FormData

const  formData = new FormData() // création d'un objet FormData vide

formData.append('username', 'Chris') // ajouter une clé/valeur à l'objet FormData
```

```javascript
  const [data, setData] = useState({})

  const handleSubmit = (event) => {
    event.preventDefault();
    
    const formData = new FormData(event.target)
    const data = formData.get("passwordPage")
    console.log(data)

    setData(formData)
  };

  return (
    <div>
      <form onSubmit={handleSubmit} name="form">
        <input type="text" name="targetPage" placeholder="Page number" />
        <input type="password" name="passwordPage" placeholder="Pass" />
        <button>Go</button>
      </form>
    </div>
  )
}
```
