# Unités de mesure CSS

## rem et la technique du 62.5%

L'élément html de base a un font-size de 16px sur tous les navigateurs

````txt
/* html font-size = 16px
   1 REM = 16px
   
   10 / 16 = 0.625

   62.5% de 16 = 10px
   */

````

````html
<div class="container">
        <h1>Mon titre</h1>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Molestias dolorem ipsum reprehenderit quas pariatur labore vitae quos molestiae dolores nemo. Eos beatae ratione modi unde dolor similique sit, sint vero!</p>
    </div>
````

````css
html {
    font-size: 62.5%;
    /* 62.5% de 16 = 10px 
       donc 1 REM = 10px*/

}
.body {
    height: 100vh;
}
h1 {
    text-align: center;
    font-size: 7rem;
    font-family: sans-serif;
}
p {
    text-align: center;
    font-size: 2rem;
    padding: 0 2.5rem;
}
.container {
    border: 10px solid black;
    width: 60%;
    margin: auto;
    position: relative;
    top: 30%;
}
@media screen and (max-width: 1200px) {
    html {
        font-size: 40%;
    }
}
````

````html
vw : 1% de la largeur du viewport de l'utilisateur

vh : 1% de la hauteur du viewport de l'utilisateur

vmin : 1% de la plus basse dimension du viewport de l'utilisateur

vmax : 1% de la plus haute dimension du viewport de l'utilisateur

Le view height est très intéressant car les éléments de block prennent la taille de leur contenu.
Les éléments Html et body sont des blocs, c'est pour ca qu'il n'est pas possible de leur donner une taille en pourcentage (100% ne fonctionne pas).
Les vh sont très intéressant pour donner une hauteur variable en fonction du viewport de l'utilisateur

````
