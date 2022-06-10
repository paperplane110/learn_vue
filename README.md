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

In html:

```html
<div id="app" v-cloak>
    <login-form />
</div>
```

In js:

> app.component(name, Object)

- name: the name of the component
- Object: a object with a `template` attribute.
  - `template`: declare the html of the component.
  - `data`: stores the data of the component.
  - `methods`: the methods of the component.

```js
let app = Vue.createApp({
    ...
})
app.component('login-form', {
template: `
            <form @submit.prevent="handleSubmit">
                <h1>{{ title }}</h1>
                <input type="email" v-model='email' />
                <input type="password" v-model='password' />
                <button type="submit">Log in</button>
            </form>
        `,
data: function () {
    return {
    title: "Login Form",
    email: '',
    password: '',
    }
},
methods: {
        handleSubmit() {
            console.log(`Submit! email is ${this.email}; pwd is ${this.password}`)
        }
    }
})
app.mount('#app')
```

> `@submit.prevent` is for preventing the default page refresh.

## 4. Component Props

Use child component in parent component:

```js
// In parent component, register the child component

app.component('parent', {
    template: `
        <child />
    `,
    components: ['child']
})

app.component('child', {
    template: `...`
})

```

Pass value from parent component to child component

```js
// Use props in child component
// Pass value through attribute

app.component('parent', {
    template: `
        <child v-bind:propA="p1" />
        or
        <child :propA="p1" />
    `,
    components: ['child'],
    data() {
        return {
            p1: "..."
        }
    }
})

app.component('child', {
    template: `
        <p>{{ propA }}</p>
    `,
    props: ['propA']
})
```

Receive value from parent and emit value to parent. Use `computed` and `set()`/`get()`

```js
app.component('parent', {
    template: `
        <child v-bind:propA="p1" v-model='p2' />
        or
        <child :propA="p1" v-model='p2' />
    `,
    components: ['child'],
    data() {
        return {
            p1: "...",
            // the value of p2 is passed to child though v-model, 
            // the attribute is 'modelValue'
            p2: "..."
        }
    }
})
app.component('child', {
    template: `
        <p>{{ propA }}</p>
        <input type="text" v-model="inputValue"/>
    `,
    // The 'modelValue' here is passed from parent's v-model.
    props: ['propA', 'modelValue'],
    computed: {
        // the name of child's attribute
        inputValue: {
            get() {
                // inputValue can get value from modelValue which is from parent.
                return this.modelValue
            },
            set(value) {
                this.$emit('update:modelValue', value)
            }
        }
    }

})
```

## Loops

![lifecycle](static/lifecycle.svg)

```js
app.component('box', {
    template: `
        <div v-if="isVisible" class="box"></div>
    `,
    props: ['isVisible'],
    created() {
        console.log('box created')
    },
    mounted() {
        console.log('box mounted')
    },
    unmounted() {
        console.log('box unmounted')
    }
})
```

When to use ***Lifecycle Hooks***?

- *Check if user is authorized
- API Calls
- Creating or removing evetns
- Getting or cleaning data
