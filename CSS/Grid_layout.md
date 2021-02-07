# Grid Layout

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
}

````

>La grille commencera par placer les éléments pour lesquels on a défini une position. Puis les autres sont placés automatiquement dans les espaces restants.

