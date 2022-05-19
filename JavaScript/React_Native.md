# React Native

## Démarage avec `Expo`

```shell
npm install -g expo-cli

expo init AwesomeProject

cd AwesomeProject
npm start 
or
expo start
```

## Correspondances

```html
// React Navigation vs react router dom v6
<NavigationContainer></NavigationContainer> vs <BrowserRouter></BrowserRouter>
<Stack.Navigator></Stack.Navigator> vs <Routes></Routes>
<Stack.Screen name="Home" component={HomeScreen} /> vs <Route path="home" element={<Home />}>
```

```html
<!--HTML et JS -->
<View> vs <div>
<Text> vs <p> 
<Image> vs <img />
<TextInput> vs <input />
<FlatList> vs .map()vsnPress vs onClick
StyleSheet.create vs fichier css
AsyncStorage vs localStorage
```

### Style

```javascript
import { Text, StyleSheet, View } from "react-native";

const App = () => {
  return (
    <View style={styles.container}>
      <Text style={[styles.title, styles.activeTitle]}>My text</Text>
    </View>
  );
};

export default App;

const styles = StyleSheet.create({
  container: {
    flex: 1,

    justifyContent: "center",
    alignItems: "center",
  },
  title: {
    fontSize: 20,
    fontWeight: "bold",
  },
  activeTitle: {
    color: "purple",
  },
});
```

### ScrollView

```html
  return (
    <ScrollView>
      <View style={{ height: 400, backgroundColor: "crimson" }}></View>
      <View style={{ height: 400, backgroundColor: "pink" }}></View>
      <View style={{ height: 400, backgroundColor: "salmon" }}></View>
      <View style={{ height: 400, backgroundColor: "white" }}></View>
      <View style={{ height: 400, backgroundColor: "deeppink" }}></View>
    </ScrollView>
  );
```

### Image

```javascript
{/* En local */}
      <Image
        source={require("./assets/cover.jpg")}
        style={styles.cover}
        resizeMode="contain"
      />

{/* Image distante : */}
      <Image
        source={{
          uri: "https://reactnative.dev/img/tiny_logo.png",
        }}
        style={styles.logo}
        resizeMode="contain"
      />
```

### TextInput

```javascript
import React, { useState } from "react";
import { TextInput } from "react-native";

function App() {
  const [email, setEmail] = useState("");
  
  return (
     <TextInput
        style={{ height: 44, borderColor: "gray", borderWidth: 1 }}
        onChangeText={text => {
          setEmail(text);
        }}
        value={email}
      />
  );
}

export default App;
```

### Button

Le composant Button est très peu stylisable avec react Native

```javascript
import { Button, StyleSheet, View } from "react-native";

      <Button
        title="Press here"
        onPress={() => {
          console.log("pressed !");
        }}
      ></Button>

```

### Button TouchableOpacity

```javascript
<TouchableOpacity
      style={styles.btn}
      activeOpacity={0.8}
      onPress={() => {
        console.log("pressed !");
      }}
    >
```

### Button TouchableHighlight

```javascript
<TouchableHighlight
      style={styles.btn}
      underlayColor="yellow"
      onPress={() => {
        console.log("pressed !");
      }}
    >
```

### ActivityIndicator

indicateur de chargement

```javascript
<ActivityIndicator size="large" color="purple" style={{ marginTop: 100 }} />
```

### SafeAreaView

Sous IOS afin déviter que le contenu se place sous les encoches.

```javascript
<SafeAreaView>
      <ScrollView>
        <View style={{ height: 500, backgroundColor: "orange" }}></View>
        <View style={{ height: 500, backgroundColor: "purple" }}></View>
        <View style={{ height: 500, backgroundColor: "yellow" }}></View>
      </ScrollView>
</SafeAreaView>
```

### KeyboardAwareScrollView

Permet de déplacer une vue pour ne pas être gêné par le clavier.

```script shell
npm i react-native-keyboard-aware-scroll-view
```

```javascript
import { KeyboardAwareScrollView } from "react-native-keyboard-aware-scroll-view";
```

### Platform

Pour savoir si le smartphone est ios ou android.

```javascript
import { Platform, StyleSheet } from "react-native";

const styles = StyleSheet.create({
  height: Platform.OS === "ios" ? 200 : 100
});
```

### Constants

Permet par exemple de savoir la hauteur de la status bar

```shell script
expo install expo-constants
```

```javascript
import Constants from 'expo-constants';

const styles = StyleSheet.create({
  scrollView: {
    paddingTop: Constants.statusBarHeight,
  },
});
```

### Dimension

Récupérer la taille de l'écran

```javascript
import { Dimensions } from 'react-native'

const width = Dimensions.get("window").width
const height = Dimensions.get("window").height
```

### Vector Icons

Installé avec expo

```javascript
import { Ionicons } from '@expo/vector-icons';

const IconExample = () => {
  return (
    <Ionicons name="md-checkmark-circle" size={32} color="green" />
  );
};

export default IconExample;
```

### FlatList

.map ultra optimisé

```javascript
<FlatList
  data={[
    { id: 1, name: "Brice" },
    { id: 2, name: "Alexis" },
    { id: 3, name: "Pierre" }
  ]}
  keyExtractor={item => String(item.id)}
  renderItem={({ item }) => <Text>{item.name}</Text>}
/>
```

### AsyncStorage

locaStorage de React Native

```script shell
expo install @react-native-async-storage/async-storage
```

```javascript
import AsyncStorage from '@react-native-async-storage/async-storage'

// enregistrement variable String
const password = "secret"
await AsyncStorage.setItem("password", password)

// récupération String
const password = await AsyncStorage.getItem("user");

// enregistrement variable non String
const value = JSON.stringify({
  name: "Farid",
  age: "32"
});

await AsyncStorage.setItem("user", value);

// récupération non String
const stored = await AsyncStorage.getItem("user");
const user = JSON.parse(stored);

```

---

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
  // ici l'option title changera le titre Home( par defaut) dans l'en-tête de Home en Overview
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

### Se déplacer entre les `screens`

```javascript
import { Button, View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

// la prop navigation est passé à tous les composants screens depuis Stack.navigator
function HomeScreen({ navigation }) { 
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
      <Button
        title="Go to Details"
        onPress={() => navigation.navigate('Details')}
      />
    </View>
  );
}

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
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
```

`navigation.push` A voir !!!

```javascript
<Button
  title="Go to Details... again"
  onPress={() => navigation.push('Details')}
/>
```

`navigation.goBack()` `navigate('Home')` `navigation.popToTop()`

```javascript
function DetailsScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
      <Button
        title="Go to Details... again"
        onPress={() => navigation.push('Details')}
      />
      <Button title="Go to Home" onPress={() => navigation.navigate('Home')} />
      <Button title="Go back" onPress={() => navigation.goBack()} />
      <Button
        title="Go back to first screen in stack"
        onPress={() => navigation.popToTop()}
      />
    </View>
  );
}
```

### Passer des paramêtres aux routes

```javascript
navigation.navigate('RouteName', { paramName: 'value' })
```

### Lire des paramêtres

```javascript
if (route.params?.post)
```

### Mettre à jour des paramêtres

```javascript
navigation.setParams({
  query: 'someText',
});
```

### Paramêtres initiaux

```javascript
<Stack.Screen
  name="Details"
  component={DetailsScreen}
  initialParams={{ itemId: 42 }}
/>
```

```javascript
import { Text, TextInput, View, Button } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

function HomeScreen({ navigation, route }) {
  React.useEffect(() => {
    if (route.params?.post) {
      // Post updated, do something with `route.params.post`
      // For example, send the post to the server
    }
  }, [route.params?.post]);

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Button
        title="Create post"
        onPress={() => navigation.navigate('CreatePost')}
      />
      <Text style={{ margin: 10 }}>Post: {route.params?.post}</Text>
    </View>
  );
}

function CreatePostScreen({ navigation, route }) {
  const [postText, setPostText] = React.useState('');

  return (
    <>
      <TextInput
        multiline
        placeholder="What's on your mind?"
        style={{ height: 200, padding: 10, backgroundColor: 'white' }}
        value={postText}
        onChangeText={setPostText}
      />
      <Button
        title="Done"
        onPress={() => {
          // Pass and merge params back to home screen
          navigation.navigate({
            name: 'Home',
            params: { post: postText },
            merge: true,
          });
        }}
      />
    </>
  );
}

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator mode="modal">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="CreatePost" component={CreatePostScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

### Bottom Tabs Navigator

Barre d'onglets en bas de l'écran qui nous permet de basculer entre différents itinéraires.

```script shell
npm install @react-navigation/bottom-tabs
```

```javascript
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

const Tab = createBottomTabNavigator();

function MyTabs() {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Home" component={HomeScreen} />
      <Tab.Screen name="Settings" component={SettingsScreen} />
    </Tab.Navigator>
  );
}
```
