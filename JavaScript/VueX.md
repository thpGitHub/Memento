# VueX

````javascript
export default new Vuex.Store({
    state: {
        month: 8,
        day: 12,
        year: 2008
    }
})
````

- Avec `$store`

````HTML
<template>
    <p>La date stockée dans Vuex est le {{ $store.state.day }}-{{ $store.state.month }}-{{ $store.state.year }}.</p>
</template>
````

- Avec `mapState`

````javascript
import { mapState } from 'vuex'

export default {
    computed: {
        ...mapState([
            'month',
            'day',
            'year'
        ])
    }
}
````

````HTML
<template>
    <p>La date stockée dans Vuex est le {{ day }}-{{ month }}-{{ year }}.</p>
</template>
````

- Avec `mapState` et un alias

````javascript
import { mapState } from 'vuex'

export default {
    computed: {
        ...mapState({
            month: 'month',
            day: 'day',
            year: 'year'
        })
    }
}
````

````HTML
<template>
    <p>La date stockée dans Vuex est le {{ day }}-{{ month }}-{{ year }}.</p>
</template>
````

- Avec `getters`

Les getters dans Vuex ont un concept similaire aux `computed properties` dans Vue.js

````javascript

export default new Vuex.Store({
    state: {
        month: 8,
        day: 12,
        year: 2008
    },
    getters: {
        date: state => {
            return state.day + '-' + state.month + '-' + state.year
        }
    }
})
````

````HTML
<template>
    <p>La date stockée dans Vuex est le {{ $store.getters.date }}.</p>
</template>
````

- Avec `mapGetters`

````javascript
import { mapGetters } from 'vuex'

export default {
    computed: {
        ...mapGetters([
            'date'
        ])
    }
}
````

````HTML
<template>
    <p>La date stockée dans Vuex est le {{ date }}.</p>
</template>
````
