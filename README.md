<p align="center"><img width="100" src="./images/logo.png" alt="Vue Messenger logo"></p>

<p align="center">
  <a href="https://travis-ci.org/fjc0k/vue-messenger"><img src="https://travis-ci.org/fjc0k/vue-messenger.svg?branch=master" alt="Build Status"></a>
  <a href="https://codecov.io/gh/fjc0k/vue-messenger"><img src="https://codecov.io/gh/fjc0k/vue-messenger/branch/master/graph/badge.svg" alt="Coverage Status"></a>
  <a href="https://github.com/fjc0k/vue-messenger/blob/master/dist/vue-messenger.min.js"><img src="https://img.shields.io/badge/minzipped%20size-966%20B-blue.svg?MIN" alt="Minified Size"></a>
  <a href="https://github.com/fjc0k/vue-messenger/blob/master/dist/vue-messenger.min.js"><img src="https://img.shields.io/badge/minified%20size-2%20KB-blue.svg?MZIP" alt="Minzipped Size"></a>
  <a href="https://www.npmjs.com/package/vue-messenger"><img src="https://img.shields.io/npm/v/vue-messenger.svg" alt="Version"></a>
  <a href="https://www.npmjs.com/package/vue-messenger"><img src="https://img.shields.io/npm/l/vue-messenger.svg" alt="License"></a>
</p>

# Vue Messenger

A series of useful enhancements to Vue component props:

- [Transform props values](#transform-props-values)
- [Listen for receiving props values](#listen-receiving-props-values)
- [Enum-type props](#enum-type-props)
- [Two-way data bindings](#two-way-data-bindings)


## Why ?

### Transform props values

- before

    ```html
    <template>
      <div>{{ normalizedCount }}</div>
    </template>
    
    <script>
    export default {
      props: {
        count: [Number, String]
      },
      computed: {
        normalizedCount() {
          return Number(this.count)
        }
      }
    }
    </script>
    ```

- after

    ```html
    <template>
      <div>{{ localCount }}</div>
    </template>
    
    <script>
    export default {
      mixins: [VueMessenger],
      props: {
        count: {
          type: [Number, String],
          transform: Number
        }
      }
    }
    </script>
    ```

### Listen for receiving props values
    
- before

    ```js
    export default {
      props: {
        count: [Number, String]
      },
      watch: {
        count(newCount, oldCount) {
          console.log(newCount, oldCount)
        }
      }
    }
    ```

- after

    ```js
    export default {
      mixins: [VueMessenger],
      props: {
        count: {
          type: [Number, String],
          on: {
            receive(newCount, oldCount) {
              console.log(newCount, oldCount)
            }
          }
        }
      }
    }
    ```

### Enum-type props

- before

    ```js
    export default {
      props: {
        size: {
          type: String,
          default: 'small',
          validator: value => ['small', 'large'].indexOf(value) >= 0
        }
      }
    }
    ```

- after

    ```js
    export default {
      mixins: [VueMessenger],
      props: {
        size: {
          type: String,
          enum: ['small', 'large']
        }
      }
    }
    ```

### Two-way data bindings

- before

    ```html
    <template>
      <input v-model="curValue" />
    </template>

    <script>
    export default {
      props: {
        value: String
      },
      computed: {
        curValue: {
          get() {
            return this.value
          },
          set(newValue) {
            this.$emit('input', newValue)
          }
        }
      }
    }
    </script>
    ```
  
- after

    ```html
    <template>
      <input v-model="localValue" />
    </template>

    <script>
    export default {
      mixins: [VueMessenger],
      props: {
        value: String
      }
    }
    </script>
    ```
<!-- 
# Install

## Package

```bash
# yarn
yarn add vue-messenger

# or, npm
npm i vue-messenger
```

## CDN

- [jsDelivr](//www.jsdelivr.com/package/npm/vue-messenger)
- [UNPKG](//unpkg.com/vue-messenger/)

Available as global `VueMessenger`. -->

<!-- # Example

```html
<template>
  <input
    v-model="localValue"
    v-show="localVisible"
  />
</template>

<script>
export default {
  props: {
    value: {
      type: [String, Number],
      transform: {
        receive: value => String(value),
        send: Number
      },
      on: {
        receive: console.log,
        send: console.log,
        change: console.log
      }
    },
    visible: {
      type: Boolean,
      sync: true
    }
  }
}
</script>
``` -->
