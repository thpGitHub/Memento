# Grid Layout

````css
.wrapper {
    display: grid;

    grid-template-columns: 200px 200px 200px;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-columns: repeat(3, 1fr);
    grid-template-columns: 2fr 1fr 1fr;
    grid-template-columns: 500px 1fr 2fr;
    grid-template-columns: 20px repeat(6, 1fr) 20px;
    grid-template-columns: repeat(5, 1fr 2fr); /*une colonne de 1fr suivie d'une colonne de 2fr, ceci répété 5 fois.*/

    grid-auto-rows: 200px;
    grid-auto-rows: minmax(100px, auto); /*un min de 100px de haut et un max automatiquement en fonction de la hauteur du contenu*/

}
````

````css
.wrapper {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 10px;
    
    grid-auto-rows: 100px; /* Les ligne seront de 100 pixels*/
    grid-auto-rows: minmax(100px, auto);
    grid-auto-rows: 100px 200px; /*1ere ligne 100px 2eme 200px*/
    grid-auto-rows: minmax(100px, auto);
}
````

````css
.wrapper {
    display: grid;
    grid-template-rows: repeat(3, 200px);
    grid-gap: 10px;
    grid-auto-flow: column;
    grid-auto-columns: 300px 100px; /*1er colonne 300 2eme 100 3eme 300*/

    grid-column-start: 1;
    grid-column-end: 4;
    grid-row-start: 1;
    grid-row-end: 3;

    grid-column-gap: 10px;
    grid-row-gap: 1em;
    grid-gap: 20px;
    gap /*Attention pas encore supporté par tous les navigateurs*/

}

````

>La grille commencera par placer les éléments pour lesquels on a défini une position. Puis les autres sont placés automatiquement dans les espaces restants.

