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
