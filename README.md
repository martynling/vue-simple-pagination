# vue-simple-pagination
A simple Vue.js pagination component. Provide it with the pageCount and the current page and it displays the pagination. 

# Requirements

- Vue.js ^`2.0.0`

# Installation
Assuming that you'll be using gulp or browserify to roll all your js into a single file:
 
```shell
$ npm install vue-simple-pagination --save-dev
```

# Usage

```javascript
var Vue = require('vue');
import VueSimplePagination from 'vue-simple-pagination'
Vue.component('vue-simple-pagination', VueSimplePagination)
```

After installing the plugin you can use it like this

```html
<vue-simple-pagination
    v-on:page-changed="fetchData"
    v-bind:page-count="pageCount"
    v-bind:current-page="currentPage"
</vue-simple-pagination>
```

```javascript
var vm = new Vue({
    el: 'body',
    data: {
        pageCount: 200,
        currentPage: 5
    },
    
    methods: {
        fetchData(selectedPage) {
            // Your AJAX or other code to display the data for the newly selected currentPage
        }
    }
});
```

## Props

- `pageCount`
- `currentPage`

## Events

 - `page-changed` - fired whenever the page is changed and passed the new page value.  
