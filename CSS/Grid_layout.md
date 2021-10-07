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

    grid-column-start: 3;
    grid-column-end: span 2; /*s'étend sur 2 colonnes depuis le start (3)*/
    
    grid-column-start: span 3; /*s'étend sur 3 colonnes depuis le end (6)*/
    grid-column-end: 6; 

    grid-column: 2/4; /*super propriété grid-column-start et grid-column-end*/
    grid-row: 3/4;

    grid-column-gap: 10px; /*espace de 10px entre les colonnes*/
    grid-row-gap: 1em; /*espace de 1em entre les lignes*/
    grid-gap: 20px; /*super propriété pour column-gap et row-gap*/
    grid-gap: 10%;
    grid-gap: 10px 20px;
    gap /*Attention pas encore supporté par tous les navigateurs*/

    grid-area 1/2/4/6; /*super propriété  grid-row-start/grid-column-start/grid-row-end/grid-column-end*/

    grid-template-columns: 20% 20% 20%;
    grid-template-rows: 20% 20% 20%;
    

}

````

>La grille commencera par placer les éléments pour lesquels on a défini une position. Puis les autres sont placés automatiquement dans les espaces restants.

---

Ici un exemple avec `auto-fill`

````css
.galerie {
  display: grid;
  grid-template-columns: repeat(auto-fill, 300px);
  justify-content: center;
  grid-gap: 10px;
}
.item {
  width: 300px;
  height: 200px;
}
.item img {
  width: 100%;
  height: auto;
}
````

---

````css
.grid3 {
  max-width: 600px;
  margin: 15px auto 150px;
  display: grid;
  grid-gap: 10px;
  grid-template-rows: repeat(3, [row] 100px);
  grid-template-columns: repeat(4, [col] 1fr);
}

.grid3 .b1 {
  grid-column: col / span 2;
}
.grid3 .b2 {
  grid-column: col 3 / span 2;
}
.grid3 .b3 {
  grid-column: col;
  grid-row: row 2
}
.grid3 .b4 {
  grid-column: col 2 / span 3;
  grid-row: row 2;
}
.grid3 .b5 {
  grid-column: col / span 4;
  grid-row: 3;
}
````
