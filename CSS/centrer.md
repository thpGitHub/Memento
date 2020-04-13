https://la-cascade.io/centrer-une-div-guide-complet/

###Centrer une div dans une div

Centrer le contenu d'une div est assez simple, il suffit de donner à la propriété ``text-align`` la valeur ``center``,

Centrer la div elle même : 
````css
    .center-div {
         margin: 0 auto;
         width: 100px; 
    // La valeur auto de la propriété margin donne aux marges gauche et droite 
    // l'espace restant disponible dans la page
    }
````

Centrer une div dans une div avec inline-block

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