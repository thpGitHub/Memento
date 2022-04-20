# Fonts

- <https://correcty.net/fr/top-5-des-convertisseurs-de-polices-en-ligne-pour-generer-font-face>
- <https://developer.mozilla.org/fr/docs/Web/CSS/@font-face>

## @font-face

`@font-face` est une astuce qui aide les utilisateurs à charger une police que notre page Web utilise.
Cela permet de ne plus dépendre des polices qui sont installées sur les postes des utilisateurs.

Pour générer le fichier css contenant la règle `@font-face` on peut utiliser le site : `- <https://transfonter.org/>`. Ce site  convertir une (ou plusieur) police par ex. .ttf et convertir cette police .ttf en .eot, .svg, .woff, .woff2 puis va générer un fichier css avec la règle @font-face.

Voici un exemple avec la police : TripAdvisor_Regular.woff2 fourni à transfonter. Transfonter nous retourne un fichier css (stylesheet.css) et les polices converties dans les formats demandé :

- TripAdvisor_Regular.eot
- TripAdvisor_Regular.svg
- TripAdvisor_Regular.ttf
- TripAdvisor_Regular.woff
- TripAdvisor_Regular.woff2

>Il faut utiliser tous ces formats de police car ils sont pris en charge par divers navigateurs Web. Par exemple, la police .eot nous aidera à charger la police dans Internet Explorer 9 et supérieur, la police .woff nous permettra de charger la police dans presque tous les navigateurs Web modernes, etc.

stylesheet.css

````css
@font-face {
    font-family: 'TripAdvisor_Regular';
    src: url('/assets/font/TripAdvisor_Regular.eot');
    src: url('/assets/font/TripAdvisor_Regular.eot?#iefix') format('embedded-opentype'),
        url('/assets/font/TripAdvisor_Regular.woff2') format('woff2'),
        url('/assets/font/TripAdvisor_Regular.woff') format('woff'),
        url('/assets/font/TripAdvisor_Regular.ttf') format('truetype'),
        url('/assets/font/TripAdvisor_Regular.svg#TripAdvisor_Regular') format('svg');
    font-weight: normal;
    font-style: normal;
    font-display: swap;
}
````

## Architecture simplifié

````text
    .
    ├── assets/
    │   ├── css/
    │       └── fonts.css   // ici sera le fichier fourni avec @font-face
    │       └── reset.css
    │       └── style.css
    │   ├── font/
    │       └── TripAdvisor_Regular.eot
    │       └── TripAdvisor_Regular.svg
    │       └── TripAdvisor_Regular.ttf
    │       └── TripAdvisor_Regular.woff
    │       └── TripAdvisor_Regular.woff2
````

Si plusieurs police (ici 2) :

````css
@font-face {
  font-family: "Trip Sans Med";
  src: url("/assets/font/TripSans-Medium.eot");
  src: url("/assets/font/TripSans-Medium.eot?#iefix")
      format("embedded-opentype"),
    url("/assets/font/TripSans-Medium.woff2") format("woff2"),
    url("/assets/font/TripSans-Medium.woff") format("woff"),
    url("/assets/font/TripSans-Medium.ttf") format("truetype"),
    url("/assets/font/TripSans-Medium.svg#TripSans-Medium") format("svg");
  font-weight: 500;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: "Trip Sans Reg";
  src: url("/assets/font/TripSans-Regular.eot");
  src: url("/assets/font/TripSans-Regular.eot?#iefix")
      format("embedded-opentype"),
    url("/assets/font/TripSans-Regular.woff2") format("woff2"),
    url("/assets/font/TripSans-Regular.woff") format("woff"),
    url("/assets/font/TripSans-Regular.ttf") format("truetype"),
    url("/assets/font/TripSans-Regular.svg#TripSans-Regular") format("svg");
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}
````