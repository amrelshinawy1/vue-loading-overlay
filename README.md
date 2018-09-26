# Vue Loading Overlay Component

[![vue-js](https://img.shields.io/badge/vue.js-2.x-brightgreen.svg?maxAge=604800)](https://vuejs.org/)
[![downloads](https://img.shields.io/npm/dt/vue-loading-overlay.svg)](http://npm-stats.com/~packages/vue-loading-overlay)
[![npm-version](https://img.shields.io/npm/v/vue-loading-overlay.svg)](https://www.npmjs.com/package/vue-loading-overlay)
[![github-tag](https://img.shields.io/github/tag/ankurk91/vue-loading-overlay.svg?maxAge=1800)](https://github.com/ankurk91/vue-loading-overlay/)
[![license](https://img.shields.io/github/license/ankurk91/vue-loading-overlay.svg?maxAge=1800)](https://yarnpkg.com/en/package/vue-loading-overlay)
[![build-status](https://travis-ci.org/ankurk91/vue-loading-overlay.svg?branch=master)](https://travis-ci.org/ankurk91/vue-loading-overlay)

Vue.js component for full screen loading indicator

## Demo on [JSFiddle](https://jsfiddle.net/ankurk91/w8y8k5wo/)

## Installation
```bash
# npm
npm install vue-loading-overlay --save

# Yarn
yarn add vue-loading-overlay
```

## Usage
#### As component
```html
<template>
    <div class="vld-parent">
        <loading :active.sync="isLoading" 
        :can-cancel="true" 
        :on-cancel="onCancel"
        :is-full-page="fullPage"></loading>
        
        <label><input type="checkbox" v-model="fullPage">Full page?</label>
        <button @click.prevent="doAjax">fetch Data</button>
    </div>
</template>

<script>
    // Import component
    import Loading from 'vue-loading-overlay';
    // Import stylesheet
    import 'vue-loading-overlay/dist/vue-loading.css';
    
    export default {
        data() {
            return {
                isLoading: false,
                fullPage: true
            }
        },
        components: {
            Loading
        },
        methods: {
            doAjax() {
                this.isLoading = true;
                // simulate AJAX
                setTimeout(() => {
                  this.isLoading = false
                },5000)
            },
            onCancel() {
              console.log("User cancelled the loader.")
            }
        }
    }
</script>
```

### As plugin
```html
<template>
    <form @submit.prevent="submit" class="vld-parent" ref="formContainer">
        <!-- your form inputs goes here-->
        <label><input type="checkbox" v-model="fullPage">Full page?</label>
        <button type="submit">Login</button>
    </form>
</template>

<script>
    import Vue from 'vue';
    // Import component
    import Loading from 'vue-loading-overlay';
    // Import stylesheet
    import 'vue-loading-overlay/dist/vue-loading.css';
    // Init plugin
    Vue.use(Loading);

    export default {
        data() {
            return {
                fullPage: false
            }
        },
        methods: {
            submit() {
                let loader = this.$loading.show({
                  container: this.fullPage ? null : this.$refs.formContainer,
                  canCancel: true,
                  onCancel: this.onCancel,
                });
                // simulate AJAX
                setTimeout(() => {
                  loader.hide()
                },5000)                 
            },
            onCancel() {
              console.log("User cancelled the loader.")
            }                      
        }
    }
</script>
```

## Available props
The component accepts these props:

| Attribute        | Type                | Default              | Description      |
| :---             | :---:               | :---:                | :---             |
| active           | Boolean             | `false`              | Show loading by default when `true`, use the `.sync` modifier to make it two-way binding |
| can-cancel       | Boolean             | `false`              | Allow user to cancel by pressing ESC or clicking outside |
| on-cancel        | Function            | `()=>{}`             | Do something upon cancel, works in conjunction with `can-cancel`  |
| transition       | String              | `fade`               | [Transition](https://vuejs.org/v2/guide/transitions.html) name |
| is-full-page     | Boolean             | `true`               | When `false`; limit loader to its container^ |
| color            | String              | ``                   | Customize the loading icon color |
| backgroundColor  | String              | ``                   | Customize the overlay background color |
| size             | Number              | ``                   | Customize the height and width of loading icon |

* ^When `is-full-page` is set to `false`, the container element should be positioned as `position: relative`. 
You can use css class `vld-parent`.

## Available slots
The component accepts these slots:

* `default` : Replace the animated icon with yours
* `text` : Place anything right after animated icon, you may need to style this.

## API methods
### `Vue.$loading.show(?propsData,?slots)`
```js
// pass propsData to component
let loader = Vue.$loading.show({
  // Optional parent container reference
  container: this.$refs.loadingContainer,
  // Can also pass available props here (camelCased property names)
  canCancel: true,// default false
  onCancel: this.yourMethodName,
  color: '#000000',
  backgroundColor: '#ffffff',
  size: 64,
},{
  // Pass slots by their name
  default: this.$createElement('your-custom-loader-component'),
});
// hide loader whenever you want
loader.hide();
```

## Global configs
You can set props and slots for all future instances when using as plugin
```js
Vue.use(Loading, {
  // props
  color: 'red'
},{
  // slots
})
```
Further you can override any prop or slot when creating new instances
```js
let loader = Vue.$loading.show({
 color: 'blue'
});
```

## Install in non-module environments (without webpack)
```html
<!-- Vue js -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5/dist/vue.min.js"></script>
<!-- Lastly add this package -->
<script src="https://cdn.jsdelivr.net/npm/vue-loading-overlay@3"></script>
<link href="https://cdn.jsdelivr.net/npm/vue-loading-overlay@3/dist/vue-loading.css" rel="stylesheet">
<!-- Init the plugin and component-->
<script>
Vue.use(VueLoading);
Vue.component('loading', VueLoading)
</script>
```

### Browser support
* [Modern](http://browserl.ist/?q=defaults%2C+not+ie+%3E+0%2Cnot+ie_mob+%3E+0) browsers only

## Run examples on your localhost
* Clone this repo
* Make sure you have node-js `>=6.10` and [yarn](https://yarnpkg.com) `>=1.x` pre-installed
* Install dependencies - `yarn install`
* Run webpack dev server - `yarn start`
* This should open the demo page at `http://localhost:9000` in your default web browser 

## Testing
* This package is using [Jest](https://github.com/facebook/jest) and [vue-test-utils](https://github.com/vuejs/vue-test-utils) for testing.
* Tests can be found in `__test__` folder.
* Execute tests with this command `yarn test`

## License
[MIT](LICENSE.txt) License
