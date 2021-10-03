# Centrer

https://la-cascade.io/centrer-une-div-guide-complet/

## Centrer une div dans une div

Centrer le contenu d'une div est assez simple, il suffit de donner à la propriété ``text-align`` la valeur ``center``,

## Centrer la div elle même

````css
    .center-div {
         margin: 0 auto;
         width: 100px; 
    // La valeur auto de la propriété margin donne aux marges gauche et droite 
    // l'espace restant disponible dans la page
    }
````

## Centrer une div dans une div avec inline-block

````css
    .outer-div {
         padding: 30px;
         text-align: center;
         // La propriété text-align ne fonctionne qu'avec les éléments inline
    }
    .inner-div {
         display: inline-block;
         padding: 50px;
    }
    
    //HTML
    
    <div class="outer-div">
    	<div class="inner-div">
        </div>
    </div>
````

## Centrer avec transform

````css
.c1 {
  position: relative;
}
.c1 h1 {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
````

## Centrer avec Flex Box

````css
.c1 {
  display: flex;
  align-items: center;
  justify-content: center;
}
````

ou

````css
.c3 {
  display: flex;
}
.c3 h1{
  margin: auto;
}
````

## Centrer avec Grid

````css
.c1 {
  display: grid;
  align-items: center;
  justify-content: center;
}
````

## Centrer avec un mixte Flex Box et Grid

````css
.c1 {
   display: grid;
}
.c1 h1 {
  display: flex;
  align-items: center;
  justify-content: center;
}
````

## Centrer avec line-height

````css
.c4 {
  text-align: center;
}
.c4 h1 {
  line-height: 400px;
  /* la hauteur de la ligne est la même que son conteneur */
}
````
