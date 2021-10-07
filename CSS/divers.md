# Divers

---

Possibilité d'utiliser la fonction css `clam()` pour resizer les fonts et de ne pas utiliser les medias queries

````css
.home h1 {
  font-size: clamp(25px, 7vw, 110px);
}
````

Autre solution avec `calc`

````css
h1 span {
  margin: 0 10px;
  font-size: calc(5vmin + 10px);
  // 
}
````

---

`object-fit` définit la façon dont le contenu d'un élément remplacé (`<img>` ou `<video>` par exemple) doit s'adapter à son conteneur en utilisant sa largeur et sa hauteur.

````css
.home {
    height: 100vh;
    position: relative;
}
video {
    position: absolute;
    width: 100%;
    height: 100%;
    object-fit: cover;
}
````

---

*Un élément positionné (position: ) va passer au dessus d'un élément non positioné !*

---

Cacher un élément

````css
/* il occupe toujours l'espace qui lui est alloué et en le laissant accessible (click, screen reader...) */
/* Vous pouvez tester le click sur les paragraphes grace au script en bas de l'index.html */

.p1 {
   opacity: 0;
}

/* Occultez le second paragraphe sans le rendre accessible. */

.p2 {
visibility: hidden;
}

/* Occultez le troisième paragraphe en faisant en sorte qu'il ne prenne plus d'espace et qu'on ne puisse pas cliquer dessus et qu'il ne soit plus accessible. */

.p3 {
    display: none;
}

````

---

Fusion des marges

avec flexbox et grid la fusion des marges n'existe plus :)

````css

````

---

Texte Responsive

---

Animation

---

Transitions

````css
.box {
  margin: 100px auto;
  width: 50px;
  height: 50px;
  background-color: salmon;
  cursor: pointer;
  transition: height 2s 0s, width 2s 1s, transform 2s 2s;
}
.box:hover {
  height: 100px;
  width: 200px;
  transform: rotate(720deg);
  }
````
