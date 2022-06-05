# material-ui

```shell script
npm install @mui/material @emotion/react @emotion/styled
# for the theme
npm install @mui/styles
# cela ne marche pas avec react 18 il faut essayer :
npm i @mui/styles --force
```

-<https://mui.com/system/styles/advanced/>

## Theming

Les composants MUI sont livrés avec un thème par défaut. Si on souhaite personnaliser le thème, on doit utiliser le `ThemeProvidercomposant` afin d'injecter un thème dans votre application.
Les thèmes nous permettent de personnaliser tous les aspects de conception de votre projet.

`ThemeProvider` s'appuie sur la fonctionnalité de contexte de React pour transmettre le thème aux composants

-<https://mui.com/material-ui/customization/theming/>

*ATTENTION* `@mui/styles` n'est pas compatible avec React.StrictMode ou React 18.

```javascript

```