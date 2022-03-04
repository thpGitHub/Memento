# HTML les bases

-
> HyperText Markup Language
> HTML se compose d'une série d'éléments
> Un élément est typiquement constitué d'une balise ouvrante ayant quelques attributs, du contenu textuel et d'une balise fermante

````html
    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="utf-8">
        <title>Ma page</title>
      </head>
      <body>     
      </body>
    </html>
````

- Images

````html
    <img src="images/mountain.png" alt="Mountain"> 
````

- Titres

````html
     <h1>Mon titre principal</h1>
     <h2>Mon titre de section</h2>
     <h3>Mon sous-titre</h3>
     <h4>Mon sous-sous-titre</h4>
````

- Paragraphes

````html
    <p>Voici un paragraphe</p>
````

- Listes

````html
    <!-- <ul> unordered list   -->
    <ul> 
          <li>tech</li> <!-- <li> list item -->
          <li>chercheurs</li>
          <li>bâtisseurs</li>
        </ul>
    
        <!-- <ol> ordered list  -->
        <ol> 
              <li>tech</li>
              <li>chercheurs</li>
              <li>bâtisseurs</li>
        </ol>
````

- Liens

````html
    <!-- href hypertext reference  -->    
    <a href="https://www.toto.fr">toto land</a>

    <!-- attribut target : 

        _self- Défaut. Ouvre le document dans la même fenêtre / onglet où il a été cliqué
        _blank - Ouvre le document dans une nouvelle fenêtre ou un nouvel onglet
        _parent - Ouvre le document dans le cadre parent
        _top - Ouvre le document dans le corps entier de la fenêtre
    --> 
    <a href="https://www.toto.fr/" target="_blank">Visit toto</a>

    <!-- lien image-->
    <a href="default.asp">
        <img src="smiley.gif" alt="HTML tutorial">
    </a>
    
    <!-- lien email-->
    <a href="mailto:someone@example.com">Send email</a>

    <!-- lien bouton -->  
    <button onclick="document.location='default.asp'">HTML Tutorial</button>
````

- Sauts de ligne

````html
    <p>This is a <br> paragraph with a line break.</p>
````

- CSS

````html
    <h1 style="color:blue;">A Blue Heading</h1>
    
    <p style="color:red;">A red paragraph.</p>
````

````html
    <!DOCTYPE html>
    <html>
        <head>
            <style>
                body {background-color: powderblue;}
                h1   {color: blue;}
                p    {color: red;}
            </style>
        </head>
        <body>
        </body>
    </html>
````

````html
    <!DOCTYPE html>
    <html>
        <head>
            <link rel="stylesheet" href="styles.css">
        </head>
        <body>
        </body>
    </html>
````

- Règles horizontales

````html
    <p>This is some text.</p>
    <hr>
    <h2>This is heading 2</h2>
````

- texte préformaté

````html
    <pre>
      My Bonnie lies over the ocean.
    
      My Bonnie lies over the sea.
    </pre>
````

- Éléments de formatage

````html
    <b> - Texte en gras
    <strong> - Texte important
    <i> - Texte italique
    <em> - Texte accentué
    <mark> - Texte marqué
    <small> - Texte plus petit
    <del> - Texte supprimé
    <ins> - Texte inséré
    <sub> - Texte en indice
    <sup> - Texte en exposant
````

- Éléments de citation

````html
    <abbr>Defines an abbreviation or acronym
    <address>Defines contact information for the author/owner of a document
    <bdo>Defines the text direction
    <blockquote>Defines a section that is quoted from another source
    <cite>Defines the title of a work
    <q>Defines a short inline quotation
````

- commentaires

````html
    <!-- This is a comment -->

    <!-- Do not display this image at the moment
    <img border="0" src="pic_trulli.jpg" alt="Trulli">
    -->
````
