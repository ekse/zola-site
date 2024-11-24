+++
title = "Vue"
date = "2024-11-22"
slug = "vue"

[taxonomies]
categories = ["Notes"]
tags = ["vue", "frontend"]

+++

# Vue

Components in Vue are named based on the filename they are in.

Attributes, named props, can be defined on components.

```js
defineProps(['my_value'])
```

The type of prop can be defined.

```ts
const props = defineProps({
  foo: { type: String, required: true },
})
```


The value is set as an attribute on the component.

```html
<MyComponent my_value="test"/>
```

