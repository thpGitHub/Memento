# Flexbox

> les blocs suivants s'affichent les uns au dessous des autres

````html
<div id="conteneur">
    <div class="element 1">Element </div>
    <div class="element 2">Element </div>
    <div class="element 3">Element </div>
</div>
````

> Avec la propriété CSS, tous les blocs se placent côte à côte

````CSS
#conteneur
{
    display: flex;
}
````

---

## flex-Direction

````css
flex-direction: row | row-reverse | column | column-reverse
/* row valeur par defaut */
````

````css
#conteneur
{
    display: flex;
    flex-direction: column-reverse;
}
````

## flex-wrap

````css
flex-wrap: nowrap | wrap | wrap-reverse
/* nowrap valeur par defaut */
````

## flex-flow

``flex-flow`` est un raccourci des propriétés ``flex-direction`` et ``flex-wrap``

````css
flex-flow: column nowrap;
````

## justify-content

La propriété `justify-content` aligne les élément le long de la ligne de l'axe principal (axe horizontal)

````css
justify-content: flex-start | flex-end | center | space-between | space-around
/* flex-start valeur par defaut*/
````

## align-items

la propriété `align-items` est similaire à ``justify-content` mais aligne les éléments le long de l'axe transversal (axe vertical perpendiculaire à l'axe principal)

````css
align-items: flex- start | flex- end | centre | ligne de base | étendue
````

## align-content

Cete propriété est utile lorsqu'il y a plusieurs lignes. Cette propriété n'a aucun effet lorsque le conteneur flex ne comporte qu'une seule ligne (une seule ligne utilser plutot justify-content).

````css
align-content: flex-start | flex-end | center | space-between | space-around | stretch
````

## order

Cette propriété modifie l'ordre d'affichage des éléments. Par defaut l'ordre de tous les éléments est à 0

````css
.element_1 {
    order: -1;
}
````

## align-self

Cette propriété permet de changer l'alignement d'un seul élément

````css
align-self: auto | flex-start | flex-end | center | baseline | stretch
````

## flex-grow

Cette propriété définit le facteur de croissance d'un élément par rapport aux autres élémnts dans le conteneur. Un facteur est un nombre. La valeur par défaut est 0 et les nombres négatifs ne sont pas autorisés.

````css
flex-grow: 2
/* l'élément grandira deux fois plus qu'un élément flex-grow: 1 il aura ddeux fois plus d'espace */
````

## flex-shrink

Cette propriété définit le facteur de réduction d'un élément par rapport aux autres éléments dans le conteneur. Un facteur est un nombre. La valeur par défaut est 1.

## flex-basis

## flex
