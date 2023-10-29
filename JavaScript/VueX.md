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

## Mutations

Les mutations sont les seules fonctions qui peuvent modifier l'état dans Vuex.

````javascript
export default new Vuex.Store({
    state: {
        month: 8,
        day: 12,
        year: 2008
    },
    mutations: {
        setDate (state, payload) {
            state.month = payload.month
            state.day = payload.day
            state.year = payload.year
        }
    }
})
````

````javascript
this.$store.commit('setDate', {
    month: 8,
    day: 12,
    year: 2008
})
````

## Actions

Les actions sont des fonctions qui peuvent appeler des mutations.

````javascript
export default new Vuex.Store({
    state: {
        month: 8,
        day: 12,
        year: 2008
    },
    mutations: {
        setDate (state, payload) {
            state.month = payload.month
            state.day = payload.day
            state.year = payload.year
        }
    },
    actions: {
        setDate (context, payload) {
            context.commit('setDate', payload)
        }
    }
})
````

````javascript
this.$store.dispatch('setDate', {
    month: 8,
    day: 12,
    year: 2008
})
````

## Modules

Les modules sont des sous-ensembles de Vuex qui peuvent être utilisés pour organiser le code.

````javascript
export default new Vuex.Store({
    modules: {
        date: {
            state: {
                month: 8,
                day: 12,
                year: 2008
            },
            mutations: {
                setDate (state, payload) {
                    state.month = payload.month
                    state.day = payload.day
                    state.year = payload.year
                }
            },
            actions: {
                setDate (context, payload) {
                    context.commit('setDate', payload)
                }
            }
        }
    }
})
````

````javascript
this.$store.dispatch('date/setDate', {
    month: 8,
    day: 12,
    year: 2008
})
````

## Getters

Les getters sont des fonctions qui peuvent être utilisées pour obtenir des données de l'état.

````javascript
export default new Vuex.Store({
    modules: {
        date: {
            state: {
                month: 8,
                day: 12,
                year: 2008
            },
            getters: {
                date: state => {
                    return state.day + '-' + state.month + '-' + state.year
                }
            },
            mutations: {
                setDate (state, payload) {
                    state.month = payload.month
                    state.day = payload.day
                    state.year = payload.year
                }
            },
            actions: {
                setDate (context, payload) {
                    context.commit('setDate', payload)
                }
            }
        }
    }
})
````

````javascript
this.$store.getters['date/date']
````

## Exemple

````javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
    modules: {
        date: {
            state: {
                month: 8,
                day: 12,
                year: 2008
            },
            getters: {
                date: state => {
                    return state.day + '-' + state.month + '-' + state.year
                }
            },
            mutations: {
                setDate (state, payload) {
                    state.month = payload.month
                    state.day = payload.day
                    state.year = payload.year
                }
            },
            actions: {
                setDate (context, payload) {
                    context.commit('setDate', payload)
                }
            }
        }
    }
})
````

````javascript
import { mapState, mapGetters } from 'vuex'

export default {
    computed: {
        ...mapState({
            month: 'date/month',
            day: 'date/day',
            year: 'date/year'
        }),
        ...mapGetters([
            'date'
        ])
    },
    methods: {
        setDate () {
            this.$store.dispatch('date/setDate', {
                month: 8,
                day: 12,
                year: 2008
            })
        }
    }
}
````

````HTML
<template>
    <p>La date stockée dans Vuex est le {{ day }}-{{ month }}-{{ year }}.</p>
    <p>La date stockée dans Vuex est le {{ date }}.</p>
    <button @click="setDate">Set date</button>
</template>
````

