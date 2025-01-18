+++
title = "Vue"
date = "2024-11-22"
slug = "vue"

[taxonomies]
categories = ["Notes"]
tags = ["vue", "frontend"]

+++

# Vue

Notes based in part on the [Framework Field Guide](https://playfulprogramming.com/collections/framework-field-guide-fundamentals).


Components in Vue are named based on the filename they are in.

### Properties

Properties, named props, can be defined on components.

```js
defineProps(['my_value'])
```

The type of a prop can be defined.

```ts
const props = defineProps({
  foo: { type: String, required: true },
})
```


The value is set as an attribute on the component.

```html
<MyComponent my_value="test"/>
```

### Reactivity

For changes to values to be reflected automatically in the UI, the values need to be wrapped with `ref()`. 
There are no changes necessary in the template.

```ts
const dateStr = ref(formatDate(new Date()));
```

When modifying the value, `.value` needs to be changed.

```ts
dateStr.value = formatDate(tomorrow);
```

Attributes of html elements can also be bound to a value. `v-bind` can be omitted.

```html
<span v-bind:aria-label="labelText"></span>
<span :aria-label="labelText"></span>
```

`v-on` is used to bind to events. The @ symbol can be used as a shortform.

```html
<button v-on:click="selectFile()"></button>
<button @click="selectFile()"></button>
```