# Overview 

Thia page showx how to create an editor with vue components.

## Parent

```html
<template lang>
  <div id="app">
    .helo {{size}} {{sizeb}}
    <doggie :size.sync="size"/>
    <doggie :size.sync="sizeb"/>
  </div>
</template>

<script>
import doggie from "./components/HelloWorld.vue";

export default {
  name: "app",
  data: function() {
    return {
      size: "Hello",
      sizeb:"Hellob"
    };
  },
  components: {
    doggie
  }
};
</script>
```

## Child

```html
<template >
<div>
      <h1>The {{size}}, {{color}} dog chased me</h1>
      <input :value="size" @input="$emit('update:size', $event.target.value)" />
    </div>
</template>

<script>
export default {
  name: 'doggie',
  props: {
    size: String,
  }
}
</script>
``` 
