# NextJS

## Méthodes de rendu

### `CSR` : Client Side Rendering

Le serveur va renvoyer un fichier HTML presque vide au navigateur, puis le navigateur va télécharger et exécuter le javascript et le css. Le navigateur va recevoir tous les fichiers et reconstruire la page Web.

Le `CSR` est utilisé par `create-react-app` et `viteJS`.

### `SSR` : Server Side Rendering

Le fichier HTML sera généré par le serveur avec toutes les données déjà fetch. Le fichier HTML va prendre plus de temps à être reçu par le client, mais il sera déjà prêt.

Le `CSR` est utilisé par NexJS.

### `SSG` : Static Site Generation

Génération de toutes les pages HTML lors du build de l'application et on va tout stocker dans un dossier.

Quand l'utilisateur va venir sur le site, il va avoir le temps de chargement d'une application CSR et les avantages d'une application SSR.

Le SSG est idéal les sites qui ont des données statiques, comme un blog, un portfolio ou autre.

### `ISR` : Incremental Static Regeneration

C'est un mix entre le SSR et le SSG. Toutes les pages HTML seront gégérés lors du build de l'application et on va tout stocker dans un dossier, avec un temps d'expiration pour chaque page. Quand le temps d'expiration est atteint, la page va être regénérée et le nouveau contenu sera affiché.

---

## Quelque définitions

- `TTFB`: Temps pour le premier octet (Time To First Byte) (TTFB) - Le temps qu'il faut pour que le client reçoive le premier octet de contenu.
- `FP` : Première peinture (First Paint) (FP) - Le moment où le premier pixel devient visible pour l'utilisateur.
- `FCP` : Première peinture de contenu (First Contentful Paint) (FCP) - Le temps qu'il faut pour que la première partie de contenu soit visible.
- `LCP` : Plus grande peinture de contenu (Largest Contentful Paint) (LCP) - Le temps qu'il faut pour que le contenu principal de la page soit chargé.
- `TTI` : Temps pour l'interactivité (Time To Interactive) (TTI) - Le moment où la page devient interactive et répond de manière fiable aux événements de l'utilisateur.
