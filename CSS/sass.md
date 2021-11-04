# SASS

````shell script
npm install -g sass

sass input.scss output.css
sass --watch input.scss output.css
````

## Extension VS Code

`Live Sass Compiler`

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
