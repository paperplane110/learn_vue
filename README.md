# Vue 3 for Beginners

## 1. Vue.js Directives

Declare a Vue app:

```js
let app = Vue.createApp({
    /**
     * data is a function return an object, which contains 
     * some key-value.
    */
    data: function() {
        return {
            greeting: "Hello Vue 3",
            isVisible: true
        }
    }
})
```

Mount a Vue app:

```html
<div id="app">
    {{greeting}}
</div>
```

```js
app.mount('#app')
```

### v-if/v-else-if/v-else

`v-if` will decide whether the element would be rendered in DOM

Example: the `div` won't be rendered since `isRendered` is false and given to `v-if`.

```html
...
<body>
    <div v-if="isRendered"></div>
</body>
<script>
    ...
    data: function() {
        return {
            isRendered: false
        }
    }
</script>
...
```

In addition, there are others confitional vue syntax `v-else-if`, `v-else`. They are just like other program language.

```html
<div v-if="isVisible" class="box"></div>
<div v-else-if="isVisible2" class="box two"></div>
<div v-else class="box three"></div>
```

### v-show

Same as `v-if`, but `v-show` is controling the `display` attribute in css. If `v-show` is false, the element will still rendered in DOM, but won't be displayed.

### v-cloak

`v-cloak` can let you control the element display before the `vue.js` is completely loaded in the page. It often be use to prevent plain vue syntax displayed on page while vue.js isn't ready

```html
<head>
    <style>
        [v-cloak] {
            display: none;
        }
    </style>
</head>
<body>
    <div id="app" v-cloak>
        ...
    </div>
</body>
...
```

## 2. Events and Methods

### v-on:Event="Method" or @Event="Method"

e.g.

```html
<button v-on:click="isVisible = !isVisible">
    Toggle Box
</button>
```

Shorthand for event:

```vue
v-on:click=... <=> @click=...
```

#### Event with method

Firstly, add method in the Vue app, then bind the method with event.

```html
<body>
    ...
    <button @click="toggleBox">Toggle Box</button>
    ...
</body>
<script>
    let app = Vue.createApp({
        data: function() {
            return {
                isVisible: false,
                ...
            }
        },
        methods: {
            toggleBox() {
                this.isVisible = !this.isVisible
            }
        }
    })
    app.mount('#app')
</script>
```

多个 modifiers 可以串行

```vue
@click.right.foobar
```

## 3. Components

