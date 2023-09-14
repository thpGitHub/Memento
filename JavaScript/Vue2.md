# Vue 2

````HTML
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1 id="app">{{ message }}</h1>

    <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
    <script>
      const app = new Vue({
        el: "#app",
        data: {
          message: "Hello Vue!",
        },
      });
    </script>
  </body>
</html>
````

## `computed property`

Une `computed property` doit être une fonction qui retourne une valeur en fonction d'autres propriétés contenue dans `data`. Les propriétés calculées sont mises en cache en fonction de leurs dépendances réactives. Ci-dessous `totalAmount` est recalculé uniquement si `costOfApples`, `costOfBananas` ou `costOfCoconuts` change.

````javascript

````javascript
const app = new Vue({
      el: '#app',
      data: {
        costOfApples: 6,
        costOfBananas: 2,
        costOfCoconuts: 8
      },
      computed: {
        totalAmount() {
          return this.costOfApples + this.costOfBananas + this.costOfCoconuts
        }
      }
    })
````

## `methods`

````javascript
<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
````

````javascript
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
````

## `watch`

Permet de surveiller les modifications apportées à une propriété de `data`, puis d'exécuter une fonction lorsque cette propriété change.

````javascript
const app = new Vue({
      el: '#app',
      data: {
        costOfApples: 6,
        costOfBananas: 2,
        costOfCoconuts: 8
      },
      computed: {
        totalAmount() {
          return this.costOfApples + this.costOfBananas + this.costOfCoconuts
        }
      },
      watch: {
        totalAmount() {
          console.log('totalAmount changed')
        }
      }
    })
````

## `v-model`

````HTML
<div id="app">
  <p>{{ message }}</p>
  <input v-model="message" />
</div>
````

````javascript
const app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
````

## `v-bind`

````HTML
<div id="app">
  <span v-bind:title="message">
    Hover your mouse over me for a few seconds
    to see my dynamically bound title!
  </span>
</div>
````

````javascript
const app = new Vue({
  el: '#app',
  data: {
    message: 'You loaded this page on ' + new Date().toLocaleString()
  }
})
````

## `directives`

`v-if` `v-else-if` `v-else` `v-show` `v-for` `v-bind` `v-on` `v-model` ...

Les directives sont des attributs spéciaux afin d'appliquer de manière réactive des effets secondaires au DOM lorsque la valeur de son expression change.

````HTML
<p v-if="seen">Now you see me</p>
````

````javascript
const app = new Vue({
  el: '#app',
  data: {
    seen: true
  }
})
````

````HTML
<div id="app">
        <ul>
            <li v-for="item in items" :key="item.id">
                {{ item.name }}
            </li>
        </ul>
    </div>
````

````javascript
const app = new Vue({
  el: '#app',
  data: {
    items: [
      { id: 0, name: 'Apple' },
      { id: 1, name: 'Banana' },
      { id: 2, name: 'Coconut' }
    ]
  }
})
````

Certaine directive peuvent prendre un argument, indiqué par un deux-points `:`.

````HTML
<a v-bind:href="url"> ... </a>
<a v-on:click="doSomething"> ... </a>
````

argument dynamique

````HTML
<a v-bind:[attributeName]="url"> ... </a>
<a v-on:[eventName]="doSomething"> ... </a>
````

modifiers

````HTML
<form v-on:submit.prevent="onSubmit"> ... </form>
````

Le modifier `prevent` indique à la directive `v-on` d'appeler `event.preventDefault()` sur l'événement.

racourcis pour `v-bind` et `v-on`

````HTML
<!-- full syntax -->
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a :[key]="url"> ... </a>
````

````HTML
<!-- full syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
````

## créer un composent

Un composent Vue comprend généralement trois sections : `<template>`, `<script>` et `<style>`. Le `<template>` contient le HTML, le `<script>` contient le JavaScript et le `<style>` contient le CSS.

````HTML
<template>
  <div>
    <h1>{{ message }}</h1>
    <button v-on:click="reverseMessage">Reverse Message</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      message: 'Hello Vue!',
    };
  },
  methods: {
    reverseMessage() {
      this.message = this.message.split('').reverse().join('');
    },
  },
}
</script>

<style scoped>
h1 {
  font-weight: normal;
}
</style>
````

importer un composent

````javascript
import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'App',
  components: {
    HelloWorld,
  },
}
````

## css dans un fichier séparé

````javascript
<style src="./style.css" scoped>
</style>
````

## `props`

````javascript
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
}
````

````HTML
<template>
  <div>
    <h1>{{ msg }}</h1>
    <button v-on:click="reverseMessage">Reverse Message</button>
  </div>
</template>
````

````javascript
import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'App',
  components: {
    HelloWorld,
  },
  data() {
    return {
      message: 'Hello Vue!',
    };
  },
}
````

````HTML
<template>
  <div id="app">
    <HelloWorld :msg="message" />
  </div>
</template>
````

validation des props

````javascript
export default {
  props: {
    // Basic type check (`null` matches any type)
    propA: Number,
    // Multiple possible types
    propB: [String, Number],
    // Required string
    propC: {
      type: String,
      required: true,
    },
    // Number with a default value
    propD: {
      type: Number,
      default: 100,
    },
    // Object with a default value
    propE: {
      type: Object,
      // Object or array defaults must be returned from
      // a factory function
      default: function() {
        return { message: 'hello' };
      },
    },
    // Custom validator function
    propF: {
      validator: function(value) {
        // The value must match one of these strings
        return ['success', 'warning', 'danger'].indexOf(value) !== -1;
      },
    },
  },
}
````
