# Publication du Dossier `components` depuis une Application Next.js

Cette note décrit les étapes pour publier le dossier `components` d'une application Next.js en tant que package npm.

## Pré-requis

- Node.js et npm installés
- Next.js et TypeScript configurés dans le projet
- SWC pour la compilation
- `fs-extra` pour réorganiser les fichiers générés

## Étapes

### 1. Configurer SWC

Créez un fichier `.swcrc` à la racine de votre projet avec le contenu suivant :

```json
{
  "jsc": {
    "parser": {
      "syntax": "typescript",
      "tsx": true
    },
    "transform": {
      "react": {
        "runtime": "automatic"
      }
    },
    "target": "es2015"
  },
  "module": {
    "type": "es6",
    "strict": true
  }
}
```

### 2. Installer les dépendances

Installez SWC et fs-extra :

```bash
npm install --save-dev @swc/core @swc/cli fs-extra
```

### 3. Configurer TypeScript

Le fichier `tsconfig.json` doit être configuré pour générer les fichiers de déclaration :

```json
{
  "compilerOptions": {
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": false,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "react",
    "incremental": true,
    "declaration": true,
    "declarationDir": "./dist/types",
    "emitDeclarationOnly": true,
    "outDir": "./dist",
    "plugins": [
      {
        "name": "next"
      }
    ],
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["src/components/**/*"],
  "exclude": ["node_modules", "dist", ".next"]
}
```

### 4. Créer un Script de Réorganisation

Créez un fichier scripts/reorganize.js pour réorganiser les fichiers générés par SWC, car SWC va créer en plus des dossiers src et components :

```javascript
const fs = require('fs-extra');
const path = require('path');

async function reorganize() {
  const srcDir = path.join(__dirname, '../dist/components/src/components');
  const destDir = path.join(__dirname, '../dist/components');

  // Ensure the destination directory exists
  await fs.ensureDir(destDir);

  // Copy files from src/components to dist/components
  await fs.copy(srcDir, destDir);

  // Remove the unnecessary src directory
  await fs.remove(path.join(__dirname, '../dist/components/src'));
}

reorganize().then(() => {
  console.log('Files reorganized successfully');
}).catch(err => {
  console.error('Error while reorganizing files:', err);
  process.exit(1);
});
```

#### 5. Mettre à Jour package.json

Mettre à jour les scripts de build et les files (fichiers qui seront contenu dans le package NPM) dans package.json :

```json
{
  "name": "unique-first-npm-package",
  "version": "0.1.8",
  "description": "A set of reusable Next.js components",
  "main": "dist/components/index.js",
  "module": "dist/components/index.js",
  "types": "dist/types/index.d.ts",
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "build:components": "swc src/components --out-dir dist/components && node scripts/reorganize.js",
    "build:types": "tsc"
  },
  "peerDependencies": {
    "next": ">=11.0.0",
    "react": ">=17.0.0",
    "react-dom": ">=17.0.0"
  },
  "devDependencies": {
    "@babel/cli": "^7.24.5",
    "@babel/core": "^7.24.5",
    "@babel/preset-env": "^7.24.5",
    "@babel/preset-react": "^7.24.1",
    "@babel/preset-typescript": "^7.24.1",
    "@swc/cli": "^0.3.12",
    "@swc/core": "^1.5.7",
    "@types/node": "^20",
    "@types/react": "^18",
    "@types/react-dom": "^18",
    "eslint": "^8",
    "eslint-config-next": "14.2.3",
    "fs-extra": "^11.2.0",
    "postcss": "^8",
    "tailwindcss": "^3.4.1",
    "typescript": "^5"
  },
  "files": [
    "dist/components",
    "dist/types",
    "README.md"
  ]
}
```

### 6. Supprimer les Anciennes Compilations

Supprimez les anciennes compilations pour éviter les conflits :

```bash
rm -rf dist
ou
Remove-Item -Recurse -Force dist
```

### 7. Compiler les Types et les Composants

Compilez à nouveau les types et les composants :

```bash
npm run build:types
npm run build:components
```

### 8. Vérifier la Structure des Fichiers Générés

```markdown
dist/
├── components/
│   ├── index.js
│   └── MyComponent.js
└── types/
    ├── index.d.ts
    └── MyComponent.d.ts
```

### 9. Publier le Package

Incrémentez la version et publiez le package :

```bash
npm version patch
npm publish
```

---

## Automatiser le nettoyage avant chaque build

### 1. Créer un script de nettoyage

```json
{
  "scripts": {
    "clean": "node scripts/clean.js",
    "build:components": "npm run clean && swc src/components --out-dir dist/components && node scripts/reorganize.js",
    "build:types": "tsc"
  }
}
```

```javascript
// scripts/clean.js
const fs = require('fs-extra');
const path = require('path');

async function clean() {
  const distDir = path.join(__dirname, '../dist');
  try {
    await fs.remove(distDir);
    console.log('Cleaned dist directory');
  } catch (err) {
    console.error('Error while cleaning dist directory:', err);
  }
}

clean();
```

### 2.  Modifier les scripts

```json
{
  "scripts": {
    "clean": "node scripts/clean.js",
    "build:components": "npm run clean && swc src/components --out-dir dist/components && node scripts/reorganize.js",
    "build:types": "tsc",
    "build:beforeNpmPublish": "npm run build:types && npm run build:components"
  }
}
```

```bash
npm run build:beforeNpmPublish
```

Cela va :

- Nettoyer le répertoire dist en supprimant les anciennes compilations.
- Compiler les types TypeScript.
- Compiler les composants avec SWC.
- Réorganiser les fichiers compilés dans la structure correcte.
