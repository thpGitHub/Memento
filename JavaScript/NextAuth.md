# NextAuth

````shell
npm install next-auth
````

Création du fichier `[...nextauth].js` dans `pages/api/auth` ainsi toutes les demandes à /api/auth/*( signIn, callback, signOut, etc.) seront automatiquement traitées par NextAuth.js.

Avec l'utilisation de prisma :

````shell
npm install @next-auth/prisma-adapter
````

<https://next-auth.js.org/providers/credentials>
