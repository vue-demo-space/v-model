# v-model

```bash
$ vue serve App.vue
```

## 单组件内

```vue
<template>
  <div>
    <input v-model="val" />
    {{ val }}
  </div>
</template>

<script>
export default {
  data () {
    return {
      val: 'hello'
    }
  }
}
</script>
```

其实就是语法糖，相当于：

```vue
<template>
  <div>
    <input :value="val" @input="handleInput"/>
    {{ val }}
  </div>
</template>

<script>
export default {
  data () {
    return {
      val: 'hello'
    }
  },
  methods: {
    handleInput (e) {
      this.val = e.target.value
    }
  }
}
</script>
```

也可以这样简写：（注意 `$event` 这个名是 vue 定的，必须这么写）

```vue
<template>
  <div>
    <input :value="val" @input="val = $event.target.value"/>
    {{ val }}
  </div>
</template>

<script>
export default {
  data () {
    return {
      val: 'hello'
    }
  }
}
</script>
```
