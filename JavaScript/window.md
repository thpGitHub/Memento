L'objet window
-
> L'objet window représente une fenêtre contenant un document DOM ; la propriété document pointe vers le document DOM chargé dans cette fenêtre.

````html
<!DOCTYPE html>
<html>
    <body onload="myFunction()">
    
        <h1>Hello World!</h1>
    
        <script>
            function myFunction() {
                alert("Page is loaded");
                // on peut aussi le tester avec un setTimeout pour voir
                //setTimeout(function(){ alert("Hello"); }, 2000);
            }
        // output : Hello World! puis affichage de l'alert Page is loaded    
        </script>
    
    </body>
</html>
````
           

