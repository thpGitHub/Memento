``<form>``
-
> Tous les attributs de la balises ``<form>`` sont optionnels.

### Il existe différente techniques pour envoyer un formulaire :

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