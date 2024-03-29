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

`computed property` sont généralement utilisés pour les transformations, le filtrage ou le formatage des données avant de les afficher dans le modèle.
Les propriétés calculées sont plus performantes que les `watch` dans les cas où le résultat dépend d'autres données réactives, car elles ne sont recalculées que lorsque cela est nécessaire.

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

Les `watch` sont utilisés pour exécuter une logique personnalisée lorsque les données changent, par exemple en effectuant une requête API ou en déclenchant des effets secondaires.
Ils sont souvent utilisés lorsque l'on réagir à des modifications de données qui ne sont pas directement liées au rendu, comme l'enregistrement de données sur un serveur ou lorsqu'un champ spécifique change.

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

## `v-model` vs `v-bind`

v-bind is primarily used for one-way data binding, while v-model is used for two-way data binding, making it a more convenient choice when dealing with form inputs or other interactive elements where you want changes in the template to update the data property and vice versa.

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

````javascript
export default {
  props: {
    // This prop must be passed in
    propA: {
      type: String,
      required: true,
    },
    // This prop has a default value
    propB: {
      type: Number,
      default: 100,
    },
    // Objects as defaults must be returned from a
    // factory function
    propC: {
      type: Object,
      default: function() {
        return { message: 'hello' };
      },
    },
    // Array defaults must be returned from a factory
    // function
    propD: {
      type: Array,
      default: function() {
        return ['Hello'];
      },
    },
    // Custom validator function
    propE: {
      validator: function(value) {
        // The value must match one of these strings
        return ['success', 'warning', 'danger'].indexOf(value) !== -1;
      },
    },
  },
}
````

## `emit`

La methode `emit` est utilisée pour envoyer des événements personnalisés d'un composant enfant à son composant parent. Cela permet aux composants enfants de communiquer avec leurs composants parents

composant parent :

````HTML
<template>
  <div>
    <child-component @custom-event="handleCustomEvent"></child-component>
    <p>Parent received: {{ message }}</p>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  data() {
    return {
      message: ''
    };
  },
  methods: {
    handleCustomEvent(data) {
      this.message = data;
    }
  }
};
</script>
````

composant enfant :

````HTML
<template>
  <button @click="emitCustomEvent">Send Custom Event</button>
</template>

<script>
export default {
  methods: {
    emitCustomEvent() {
      this.$emit('custom-event', 'Hello from child component!');
    }
  }
};
</script>
````

## `event bus`

permet de communiquer entre des composants qui peuvent ne pas avoir de relation parent-enfant directe.
Le bus d'événements est généralement implémenté en tant qu'instance Vue, qui agit comme un bus d'événements global partagé. Les composants peuvent émettre des événements et écouter des événements émis par d'autres composants.

````javascript
// Create a new Vue instance as an event bus
const EventBus = new Vue();

// Export the event bus so that it can be imported and used across your components
export default EventBus;
````

Émettre des événements :
Dans n'importe quel composant où vous souhaitez envoyer un événement, vous pouvez utiliser le bus d'événements pour émettre l'événement :

````HTML
<script>
import EventBus from './EventBus'; // Import the event bus

export default {
  methods: {
    sendDataToOtherComponent() {
      // Emit a custom event with data
      EventBus.$emit('custom-event-name', { someData: 'Hello from Component A' });
    }
  }
};
</script>
````

Écoutez les événements :
Dans un autre composant, vous pouvez écouter l'événement émis et y réagir :

````HTML
<script>
import EventBus from './EventBus'; // Import the event bus

export default {
  data() {
    return {
      receivedData: ''
    };
  },
  created() {
    // Listen for the custom event
    EventBus.$on('custom-event-name', (data) => {
      this.receivedData = data.someData;
    });
  }
};
</script>
````

Désormais, lorsque l'événement "custom-event-name" est émis dans le premier composant, le deuxième composant recevra les données et mettra à jour son état en conséquence.

Veuillez noter que même si le modèle de bus d'événements peut être utile pour gérer la communication entre les composants, il peut également rendre plus difficile le suivi du flux de données et la compréhension des relations entre les composants dans des applications plus vastes. Par conséquent, il est généralement recommandé d'utiliser Vuex pour la gestion d'état dans des scénarios plus complexes ou d'envisager d'autres formes de communication entre composants, telles que des accessoires et des événements personnalisés, lorsque vous travaillez avec des relations parent-enfant.

## life cycle hooks

````javascript
export default {
  name: 'HelloWorld',
  data() {
    return {
      message: 'Hello Vue!',
    };
  },
  beforeCreate() {
    console.log('beforeCreate');
  },
  created() {
    console.log('created');
  },
  beforeMount() {
    console.log('beforeMount');
  },
  mounted() {
    console.log('mounted');
  },
  beforeUpdate() {
    console.log('beforeUpdate');
  },
  updated() {
    console.log('updated');
  },
  beforeDestroy() {
    console.log('beforeDestroy');
  },
  destroyed() {
    console.log('destroyed');
  },
}
````

## `slot`

Un `slot` est un template qui permet à partir d'un composant parent de passer du contenu.
Il y a deux style de `slot` : par defaut et nommé.

- `slot` par defaut

````HTML
<!-- ChildComponent.vue -->
<template>
  <div>
    <h2>Child Component</h2>
    <slot></slot>
  </div>
</template>
````

````HTML
<!-- ParentComponent.vue -->
<template>
  <div>
    <h1>Parent Component</h1>
    <child-component>
      <p>This content goes into the default slot.</p>
    </child-component>
  </div>
</template>
````

- `slot` nommé

````HTML
<!-- ChildComponent.vue -->
<template>
  <div>
    <h2>Child Component</h2>
    <slot name="header"></slot>
    <slot name="content"></slot>
  </div>
</template>

````

````HTML
<!-- ParentComponent.vue -->
<template>
  <div>
    <h1>Parent Component</h1>
    <child-component>
      <template v-slot:header>
        <h3>Header Slot Content</h3>
      </template>
      <template v-slot:content>
        <p>Content Slot Content</p>
      </template>
    </child-component>
  </div>
</template>
````

## Dynamic Components

Fonctionnalité qui permet de basculer dynamiquement entre différents composants. Ceci est utile lorsque l'on souhaite  restituer de manière conditionnelle différents composants en fonction de certains critères, tels que les interactions utilisateur ou la logique basée sur les données.
Ils sont particulièrement utiles dans les scénarios dans lesquels vous devez créer des interfaces complexes avec différents composants ou lorsque vous souhaitez créer des formulaires ou des assistants dynamiques, entre autres cas d'utilisation.

````HTML
<template>
  <div>
    <button @click="toggleComponent">Toggle Component</button>
    <component :is="currentComponent"></component>
  </div>
</template>

<script>
import ComponentA from './ComponentA.vue'; // Import ComponentA.vue
import ComponentB from './ComponentB.vue'; // Import ComponentB.vue

export default {
  data() {
    return {
      currentComponent: 'ComponentA',
    };
  },
  components: {
    ComponentA, // Register ComponentA
    ComponentB, // Register ComponentB
  },
  methods: {
    toggleComponent() {
      // Toggle between ComponentA and ComponentB
      this.currentComponent = this.currentComponent === 'ComponentA' ? 'ComponentB' : 'ComponentA';
    },
  },
};
</script>
````

## Call API

````HTML
<template>
  <div>
    <img :src="catImage" alt="cat image" />
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "CallApi",

  data() {
    return {
      catImage: null,
    };
  },

  mounted() {
    axios
      .get("https://api.thecatapi.com/v1/images/search")
      .then((response) => {
        console.log(response);
        this.catImage = response.data[0].url;
      })
      .catch((error) => {
        console.log(error);
      });
  },

  methods: {},
};
</script>
````

## Form

````HTML
<template>
  <div class="container">
    <form>
      <div class="form-group">
        <label for="firstname">Prénom</label>
        <input
          type="text"
          class="form-control"
          id="firstname"
          placeholder="Prénom"
          v-model="formDatas.firstname"
        />
      </div>
      <div class="form-group">
        <label for="text">Ton texte</label>
        <textarea
          class="form-control"
          id="text"
          v-model="formDatas.text"
        ></textarea>
      </div>

      <h3>checkbox</h3>

      <div class="form-check">
        <input
          class="form-check-input"
          type="checkbox"
          value="fraise"
          id="fraise"
          v-model="formDatas.checkFruits"
        />
        <label class="form-check-label" for="fraise"> Fraise </label>
      </div>
      <div class="form-check">
        <input
          class="form-check-input"
          type="checkbox"
          value="pomme"
          id="pomme"
          v-model="formDatas.checkFruits"
        />
        <label class="form-check-label" for="pomme"> Pomme </label>
      </div>
      <div class="form-check">
        <input
          class="form-check-input"
          type="checkbox"
          value="banane"
          id="banane"
          v-model="formDatas.checkFruits"
        />
        <label class="form-check-label" for="banane"> Banane </label>
      </div>

      <h3>Select Box</h3>

      <div class="card p-3">
        <select v-model="formDatas.select">
          <option v-for="(pays, index) in formDatas.listPays" :key="index">
            {{ pays }}
          </option>
        </select>
      </div>

      <button @click.prevent="submitForm" class="btn btn-primary mt-3">Submit</button>
    </form>

    <br />

    <div class="card p-3" v-if="infosSubmit">
      <h2>Résultat</h2>
      <p>Prénom : {{ formDatas.firstname }}</p>
      <p style="white-space: pre">Texte : {{ formDatas.text }}</p>
      <p>Fruits :</p>
      <li v-bind:key="index" v-for="(fruit, index) in formDatas.checkFruits">
        {{ fruit }}
      </li>
      <p>Pays : {{ formDatas.select }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "MyForm",

  data() {
    return {
      formDatas: {
        firstname: "",
        text: "",
        checkFruits: [],
        select: "",
        listPays: [
          "France",
          "Espagne",
          "Italie",
          "Allemagne",
          "Portugal",
          "Angleterre",
        ],
      },
      infosSubmit: false,
    };
  },

  mounted() {},

  methods: {
    submitForm() {
      this.infosSubmit = true;
    },
  },
};
</script>
````
