# NextJS

## Méthodes de rendu

### `CSR` : Client Side Rendering

Le serveur va renvoyer un fichier HTML presque vide au navigateur, puis le navigateur va télécharger et exécuter le javascript et le css. Le navigateur va recevoir tous les fichiers et reconstruire la page Web.

Le `CSR` est utilisé par `create-react-app` et `viteJS`.

### `SSR` : Server Side Rendering

Le fichier HTML sera généré par le serveur avec toutes les données déjà fetch. Le fichier HTML va prendre plus de temps à être reçu par le client, mais il sera déjà prêt.

Le `SSR` est utilisé par NexJS.

### `SSG` : Static Site Generation

Génération de toutes les pages HTML lors du build de l'application et on va tout stocker dans un dossier.

Quand l'utilisateur va venir sur le site, il va avoir le temps de chargement d'une application CSR et les avantages d'une application SSR.

Le `SSG` est idéal les sites qui ont des données statiques, comme un blog, un portfolio ou autre.

Pour utiliser le `SSG` avec des routes statiques il faut utiliser la fonction `getStaticProps` à partir d'une page. Lorsque les routes sont dynamiques il faut en plus de `getStaticProps` utiliser la fonction `getStaticPaths` pour définir une liste de tous les chemins à générer statiquement.

````typescript
// pages/posts/[id].js

// Generates `/posts/1` and `/posts/2`
export async function getStaticPaths() {
  return {
    paths: [{ params: { id: '1' } }, { params: { id: '2' } }],
    fallback: false, // can also be true or 'blocking'
  }
}

// `getStaticPaths` requires using `getStaticProps`
export async function getStaticProps(context) {
  return {
    // Passed to the page component as props
    props: { post: {} },
  }
}

export default function Post({ post }) {
  // Render post...
}
````

### `ISR` : Incremental Static Regeneration

C'est un mix entre le `SSR` et le `SSG`. Toutes les pages HTML seront gégérés lors du build de l'application et on va tout stocker dans un dossier, avec un temps d'expiration pour chaque page. Quand le temps d'expiration est atteint, la page va être regénérée et le nouveau contenu sera affiché.

---

## L'Hydratation

L'hydratation avec le SSR, va venir rendre notre application dans une "string" avec la méthode `renderToString` de React. Cette fonction va retourner le HTML au client. Pour hydrater le componsant et lui donner de "l'interactivité" il faut utiliser la methode de React `hydrateRoot`, cette methode va "relier" notre HTML du DOM à notre composant React.

Par défaut, Next.js pré-rend chaque page. Cela signifie que Next.js génère du HTML pour chaque page à l'avance , au lieu de tout faire par JavaScript côté client. Le pré-rendu peut améliorer les performances et le référencement .

Chaque code HTML généré est associé à un minimum de code JavaScript nécessaire pour cette page. Lorsqu'une page est chargée par le navigateur, son code JavaScript s'exécute et rend la page entièrement interactive. (Ce processus est appelé hydratation.)

---

## Deux formes de pré-rendu

Next.js a deux formes de pré-rendu : la génération statique et le rendu côté serveur . La différence réside dans le moment où il génère le code HTML d'une page.

- La génération statique est la méthode de pré-rendu qui génère le code HTML au moment de la construction . Le code HTML pré-rendu est ensuite réutilisé à chaque requête.

  La génération statique peut être effectuée avec et sans données.La génération statique avec données utilise la méthode `getStaticProps`

  `getStaticProps` s'exécute au moment de la construction en production, et en mode développement, getStaticProps s'exécute à chaque requête.
  À l'intérieur de la fonction, vous pouvez récupérer des données externes et les envoyer en tant qu'accessoires à la page.

  ````typescript
  export default function Home(props) { ... }

  export async function getStaticProps() {
    // Get external data from the file system, API, DB, etc.
    const data = ...

    // The value of the `props` key will be
    //  passed to the `Home` component
    return {
      props: ...
    }
  }
  ````

- Le rendu côté serveur est la méthode de pré-rendu qui génère le code HTML à chaque requête. utilisation de la requête `getServerSideProps`

````typescript
export async function getServerSideProps(context) {
  return {
    props: {
      // props for your component
    },
  };
}
````

`getServerSideProps` est appelé au moment de la demande, son paramètre ( context) contient des paramètres spécifiques à la demande (params, req, res, query ...).

- `params` : Si cette page utilise une route dynamique , params contient les paramètres de la route. Si le nom de la page est [id].js, alors params ressemblera à { id: ... }.

`getServerSideProps` doit renvoyer un objet avec l'une de ces propriétés : `props`, `notFound`, `redirect`

L'équipe derrière Next.js a créé un hook React pour la récupération de données appelé `SWR` . Nous le recommandons vivement si vous récupérez des données côté client. Il gère la mise en cache, la revalidation, le suivi de la mise au point, la récupération à intervalle, etc.

---

## Le Routing

Il faut créer un fichier dans le dossier pages.

Si ont créé `pages/users.tsx` la route sera `localhost:3000/users`

`pages/index.js` la route sera `/`

### `Link`

Le composent `<Link></Link>` importé de `next/link` permet de gérer la navigation entre les pages côté client.

````typescript
import Link from 'next/link';

export default function User() {
  return (
    <div className="p-4">
      <Link href="/" passHref>
        <a>{'<-'} Back</a>
      </Link>
      <h1>User page !</h1>
    </div>
  );
}
````

Avec les versions 12.2 et supérieures il n'est plus obligatoire d'encapsuler `<a></a>`

````typescript
import Link from 'next/link';

export default function FirstPost() {
  return (
    <>
      <h1>First Post</h1>
      <h2>
        <Link href="/">Back to home</Link>
      </h2>
    </>
  );
}
````

### Routing dynamique

Toujours dans le dossier pages avec cette syntaxe : `[nom].tsx`.

exemple : `pages/users/[id].tsx` la route sera : `localhost:3000/users/1`

`useRouter` (version 12, version 13 c'est différent)

````typescript
import Link from 'next/link';
import { useRouter } from 'next/router';

export default function User() {
  const router = useRouter();
  const { username } = router.query;

  return (
    <div className="p-4">
      <Link href="/" passHref>
        <a>{'<-'} Back</a>
      </Link>
      <h1>Hello {username}</h1>
    </div>
  );
}
````

`router.push('/users/1')` : Permet de naviguer vers la page /users/1

`router.back()` : Permet de revenir en arrière

`router.query` : Permet de récupérer les paramètres de la route

````typescript
import type { NextPage } from 'next';
import Link from 'next/link';
import { useRouter } from 'next/router';
import { FormEvent } from 'react';

const Home: NextPage = () => {
  const router = useRouter();

  const handleSubmit = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();

    const formData = new FormData(e.currentTarget);

    const username = String(formData.get('username')) ?? '';

    router.push(`/users/${username}`);
  };

  return (
    <div className="flex flex-col items-center gap-4 m-4">
      <h1 className="text-4xl">NextReact</h1>
      <p>Template</p>
      <form onSubmit={handleSubmit}>
        <input name="username" placeholder="Username" />
        <button type="submit">Submit</button>
      </form>
    </div>
  );
};

export default Home;

````

---

## Le `SSR` avec `NextJS`

On utilise la fonction `getServerSideProps` pour récupérer des props côté serveur qu'on va ensuite passer à notre composant.

````typescript
type PageProps = {
  text: string
}

// Notre page React envoyée à l'utilisateur
export default function Page({ text }: PageProps) {
  return <div>{text}</div>
}

// On récupère les props côté serveur
// Ce code n'est jamais envoyé au client !
// Il est exécuté côté serveur et les props sont envoyés au composant (donc au client)
export const getServerSideProps: GetServerSideProps<
  ComponentProps
> = async (context) => {
  // Récupérer les posts et les passer en props
  return {
    props: {
      text: "Some text from the server"
    },
  };
};
````

`getServerSideProps` helper :

- `notFound` : Si l'utilisateur n'existe pas, on renvoie une 404
- `redirect` : Si l'utilisateur n'existe pas, on redirige vers une autre page

````typescript
export const getServerSideProps: GetServerSideProps<
  ComponentProps,
  { postId: string }
> = async (context) => {
  const user = await getUser(context.params.userId);

  if (!user) {
    return {
      notFound: true,
    };
  }

  return {
    props: {
      user
    },
  };
};
````

---

## API Routes

Il faut créer une fonction dans `pages/api`

````typescript
// pages/api/hello.js
export default function handler(req, res) {
  res.status(200).json({ text: 'Hello' });
}
````

`http://localhost:3000/api/hello`

````typescript
{
  "text": "Hello"
}
````

---

## `Lazy loading`

La fonction `lazy` de react ne permet pas de pre-render les componsants côté serveur.

````typescript
const LazyComponent = lazy(() => import('./LazyComponent'))
````

NextJS a créé un wrapper de `lazy` : `next/dynamic`  qui permet de pre-render le composant côté serveur.

````typescript
import dynamic from 'next/dynamic'

const DynamicHeader = dynamic(() => import('../components/header'), {
  loading: () => <p>Loading...</p>,
})

export default function Home() {
  return <DynamicHeader />
}
````

---

## Server Components

Nouveauté NextJS en 2023. `Les Server Components` sont entièrement run côté serveur et le code ne sera jamais `intéractif` côté client. On ne peux pas utiliser les events, window, location etc... avec les `Server Components` il faut utiliser les `Client Components`.

Pour mettre à jour un `Server Components` on peut utiliser la fonction `refresh` de `next/navigation` :

## Client Components

Les `Client Components` sont des composants React, mais ils n'ont pas accès directement aux datas de la base de données, c'est le role des `Server Components. ils peuvent utiliser les events, window, location etc...  

Pour différencier un `client component` et un `server component`, il faut utiliser la directe `"use client";` au début du fichier !

````typescript
"use client";
````

---

## Routing NextJS 13

---

## Annexes

## Quelques définitions

- `TTFB`: Temps pour le premier octet (Time To First Byte) (TTFB) - Le temps qu'il faut pour que le client reçoive le premier octet de contenu.
- `FP` : Première peinture (First Paint) (FP) - Le moment où le premier pixel devient visible pour l'utilisateur.
- `FCP` : Première peinture de contenu (First Contentful Paint) (FCP) - Le temps qu'il faut pour que la première partie de contenu soit visible.
- `LCP` : Plus grande peinture de contenu (Largest Contentful Paint) (LCP) - Le temps qu'il faut pour que le contenu principal de la page soit chargé.
- `TTI` : Temps pour l'interactivité (Time To Interactive) (TTI) - Le moment où la page devient interactive et répond de manière fiable aux événements de l'utilisateur.

---

## bibliothèque `clsx` pour basculer les classes CSS

````CSS
/* alert.module.css */

.success {
  color: green;
}
.error {
  color: red;
}
````

````typescript
import styles from './alert.module.css';
import { clsx } from 'clsx';

export default function Alert({ children, type }) {
  return (
    <div
      className={clsx({
        [styles.success]: type === 'success',
        [styles.error]: type === 'error',
      })}
    >
      {children}
    </div>
  );
}
````
