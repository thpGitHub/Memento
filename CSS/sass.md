# SASS

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
