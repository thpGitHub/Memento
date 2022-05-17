# React Native

## React Navigation

```shell script
npm install @react-navigation/native

# Si utilisation d'expo :
expo install react-native-screens react-native-safe-area-context
# Si utilisation de React Native :
npm install react-native-screens react-native-safe-area-context

#Installation bibliothèque native stack
npm install @react-navigation/native-stack
```

### `NavigationContainer` doit envelopper toute l'application

```javascript
import { NavigationContainer } from '@react-navigation/native';

export default function App() {
  return (
    <NavigationContainer>
        {/* Rest of your app code */}
    </NavigationContainer>
  );
}
```

### React Native n'a pas d'API intégrée pour la navigation comme le fait un navigateur Web

`React Navigation` avec la methode `createNativeStackNavigator()` permet à notre application de passer d'un écran à l'autre et de gérer l'historique de navigation.

```shell script
npm install @react-navigation/native-stack
```

```javascript
import { View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
```

### `stack.Navigator`

```javascript
function DetailsScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home"> {/*** stack.Navigator avec une route initiale***/}
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

### Les `options`

```javascript
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={{ title: 'Overview' }}
/>
```

Il est possible de passer les même options à tous les `screens`, il faut passer la props `screenOptions` au `Stack.Navigator`

### Passer des props

Deux techniques :

- `React context` (recommandé par React)

- `render callback`

```javascript
<Stack.Screen name="Home">
  {props => <HomeScreen {...props} extraData={someData} />}
</Stack.Screen>
```
