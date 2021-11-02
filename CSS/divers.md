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
/* object-position: center */
````

Exemple avec une image

````css
.galerieImg {
    /*Voir l'exemple avec les exercices GRID*/
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
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

---

Pseudo-éléments

````html
<button class="focus-anim">
      FOCUS
    </button>
````

````css
body {
    background: #f1f1f1;
}

.focus-anim {
  padding: 30px 45px;
  font-size: 50px;
  border-radius: 3px;
  position: absolute;
  top: 40%;
  left: 50%;
  transform: translate(-50%, -50%);
  cursor: pointer;
  background: transparent;
  outline: none;
  border: none;
  color: #f1f1f1;
}
.focus-anim::before, .focus-anim::after {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transition: transform 0.2s ease-in-out;
}
.focus-anim::before {
  border: 2px solid tomato;
}
.focus-anim::after {
  background: #000;
  z-index: -1;
}
.focus-anim:hover::before {
  transform: scaleY(1.1) scaleX(1.05);
}
.focus-anim:hover::after {
  transform: scaleY(0.9) scaleX(0.95);
}

/* Lorque utilisateur appuis sur tab */

.focus-anim:focus::before {
  transform: scaleY(1.1) scaleX(1.05);
}
.focus-anim:focus::after {
  transform: scaleY(0.9) scaleX(0.95);
}

/* Lorque utilisateur click sur le button (click css) */

.focus-anim:active::before {
  transform: scaleY(1.3) scaleX(1.2);
}
````

---

Responsive

Images adaptatives directement dans le html :

````html
<img 
    srcset="
    ressources/waterfall-400.png 400w,
    ressources/waterfall-700.png 700w,
    ressources/waterfall-1100.png 1100w,
    "
    sizes="
    (max-width: 500px) 400px,
    (max-width: 800px) 700px,
    1100px
    "
    src="ressources/waterfall-1100.png" 
    alt="waterfall"
    >
````

sans media queries avec la création d'un interval avec max-width et min-width :

````css
.card {
    max-width: 1100px;
    min-width: 300px;
}
````

media queries avec un and :

````css
@media screen and (min-width: 800px) and (max-width: 1300px) {
    .txt-input {
        width: 600px;
    }
}
````

media queries en fonction de l'orientation :

````css
@media screen and (orientation: portrait) {
    .txt-input {
        background: crimson;
    }
}
````

---

Perspective

---

Parallax

````css
.section {
  height: 100vh;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  /* parallax */
  background-attachment: fixed;
  /* parallax */
  display: flex;
  justify-content: center;
  align-items: center;
}
.section h2 {
  text-align: center;
  background: #f1f1f1;
  padding: 20px;
  font-size: 55px;
  font-weight: 200;
}

.s1 {
  background-image: url(imgs/img1.jpg);
}
.s2 {
  background-image: url(imgs/img2.jpg);

}
.s3 {
  background-image: url(imgs/img3.jpg);
}
````

---

Shadows

---

Unités :

A voir rem em vmin...
A voir aussi la technique du 

````css
html {
    font-size: 62.5%;
}
````

---

Carousel

````html
<div class="cadre">
      <div class="carousel">
        <div class="element">"Exceptionnel."</div>
        <div class="element">"20/20."</div>
        <div class="element">"Service d'exception.".</div>
        <div class="element">"Ponctuel..".</div>
        <div class="element">"Exceptionnel.".</div>
      </div>
    </div>
````

````css
body {
    height: 100vh;
    font-family: sans-serif;
}

.cadre {
    position: relative;
    background: rgb(55, 55, 55);
    color: whitesmoke;
    width: 600px;
    height: 200px;
    margin: 0 auto;
    top: 10rem;
    overflow: hidden;
    border-radius: 5px;
}
.carousel {
    text-align: center;
    animation: carousel 8s ease-in-out infinite;
}
.element {
    font-size: 50px;
    line-height: 200px;
}

@keyframes carousel {
    0%, 20% {
        transform: translateY(0);
    }
    25%, 45% {
        transform: translateY(-200px);
    }
    50%, 70% {
        transform: translateY(-400px);
    }
    75%, 95% {
        transform: translateY(-600px);
    }
    100% {
        transform: translateY(-800px);
    }
}
````
