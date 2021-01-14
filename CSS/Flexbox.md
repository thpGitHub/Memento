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
