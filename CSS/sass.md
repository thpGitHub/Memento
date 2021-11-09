# SASS

````shell script
npm install -g sass

sass input.scss output.css
sass --watch input.scss output.css
````

## Extension VS Code

`Live Sass Compiler`

*!* Au 07/11/2021 live sass compiler ne fonctionne pas avec `@use` j'utilise donc `sass --watch input.scss output.css` et cela fonctionne bien avec `@use` et `@foward`

---

## Encodage

Afin d’éviter tout problème lié à l’encodage des caractères, il est recommandé de forcer l’encodage UTF-8 dans le fichier principal. Assurez-vous que ce soit le premier élément de la feuille de style et qu’il n’y ait aucun caractère de quelque nature en amont.

````css
@charset 'utf-8';
````

---

## map

la propriété map correspond aux tableaux associatifs (array). Une map est une liste de clés avec des valeurs associés.

fonctions de map :

- `map-get($map, $cle)` Retourne la valeur associée à une clé donnée d’une map.

- `map-has-key($map, $cle)` Vérifie si une clé existe dans une map.

- `map-merge($map1, $map2)` Fusionne deux maps dans une nouvelle map.

- `map-remove($map, $cle(s))` Retourne une nouvelle map tout en enlevant une ou plusieurs clés.

- `map-keys($map)` Retourne une liste des clés d’une map.

- `map-values($map)` Retourne une liste des valeurs d’une map.

````scss
$map: (
  cle1: valeur1,
  cle2: valeur2,
  cle3: valeur3
);

.block {
  font-size: map-get($map, cle1);
}

.block {
  @if map-has-key($map, cle1) {
    font-size: map-get($map, cle1);
  } @else {
    font-size : 12px;
  }
}
````

---

## Breakpoints

````scss
// Map des breakpoints
$breakpoints: (
  $small: 576px,
  $medium: 768px,
  $large: 992px,
);
// Mixin pour appeler les breakpoints facilement
@mixin bp($breakpoint) {
    @if map-has-key($breakpoints, $breakpoint) {
        $value: map-get($breakpoints, $breakpoint);
         
        @media screen and (min-width: $value) {
            @content;
        }
    }
}
// Dans le SASS
.block {
  display: none;
  @include bp(large) {
    display: block;
  }
}
````

---

## Partial (partiel)

Un partiel est un fichier Sass nommé avec un underscore au début du fichier.
Exemple `_partial.scss`,
l'underscore indique à Sass que le fichier n'est qu'un fichier partiel et qu'il ne doit pas générer un fichier CSS. Les partiels Sass sont utilisés avec `@use`.

---

## @import @use @foward

*!* il est maintenant recommander de ne plus utiliser `@import`

````scss
// _colors.scss

$red: red;
$blue: blue
````

Avec `@import`

````scss
// style.scss

@import './colors';

body {
  color: $blue;
}
````

Avec `@use`

````scss
@use './colors';

body {
  color: colors.$blue;
}
````

````scss
@use './colors' as *;

body {
  color: $blue;
}
````

---

## Variables

````scss
$base-color: #c6538c;
$border-dark: rgba($base-color, 0.88);

.alert {
  border: 1px solid $border-dark;
}
````

---

## Le nesting

````scss
.container {
  background: #00308f;

  h1 {
    font-size: 40px;
  }

  button {
    padding: 10px 15px;
    
    &:hover {
      background: lightblue;
      color: #f1f1f1;
    }
  }
}
````

## Placeholder Selectors

````scss
%title {
  font-size: 45px;
  text-align: center;
  padding: 12px 0;
}

h2 {
  color: crimson;
  @extend %title;
}
h3 {
  color: lightgreen;
  @extend %title;
}
h4 {
  color: lightblue;
  @extend %title;
}
````

---

## Mixin

````scss
@mixin inputs($padding, $fontSize, $maxWidth) {
  display: block;
  padding: $padding;
  font-size: $fontSize;
  width: 100%;
  max-width: $maxWidth;
  margin: 50px auto;
}

.inp-large {
  @include inputs(24px, 20px , 800px);
}

.inp-small {
  @include inputs(4px, 10px , 200px);
}
````

---

## Boucle

````scss
@for $i from 1 through 100 {
  .b#{$i}{
    width: 10px * $i;
    height: 10px * $i;
  }
}
````

````html
<div class="blocks-container">
      <div class="block b1"></div>
      <div class="block b2"></div>
     <!-- . 
          .
          .-->
      <div class="block b100"></div>
    </div>
````

exemple d'animation css avec une boucle sass

````css
h1 span {
  margin: 0 10px;
  font-size: calc(5vmin + 10px);
  color: #484848;
  animation: flash 1.2s linear infinite;
}
@keyframes flash {
  0% {
    color: #fff900;
    text-shadow: 0 0 7px, 0 0 50px #ff6c00;
    
  }
  90% {
    color: #484848;
    text-shadow: none;
  }
  100% {
    color: #484848;
    text-shadow: none;
  }
}

@for $i from 1 through 10 {
  h1 span:nth-child(#{$i}){
    animation-delay: ($i / 10) + s;
  }
}
````

````html
<h1>
      <span>C</span>
      <span>A</span>
      <span>R</span>
      <span>O</span>
      <span>L</span>
      <span>I</span>
      <span>N</span>
      <span>E</span>
      <span>.</span>
      <span>.</span>
    </h1>
````

---

## Architecture

 Exemple d'une architecture 7-1 (7 dossiers - 1 fichier)

````text
    .
    ├── abstracts/
    ├── base/
    ├── components/
    ├── layout/
    ├── pages/
    ├── themes/
    ├── vendors/
    └── main.scss
````

---

## Annexes

-<https://sass-guidelin.es/fr/>
