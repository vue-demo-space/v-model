# v-model

```bash
$ vue serve App.vue
```

参考：

* [v-model](https://cn.vuejs.org/v2/api/#v-model)
* [表单输入绑定](https://cn.vuejs.org/v2/guide/forms.html)
* [自定义事件](https://cn.vuejs.org/v2/guide/components-custom-events.html)

## v-model 单组件内

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

代码在这个 [commit](https://github.com/vue-demo-space/v-model/tree/9553d779917941ecc61b7ac8f0fb0219559c4ae3)

## v-model 父子组件

有时候父组件要给子组件传值，同时子组件会修改这个值。我们不能直接修改这个值，而应该在父组件上修改。用 props down，emit up 去实现

父组件：

```vue
<template>
  <div>
    <Child :value="val" @input="val = $event" />
    {{ val }}
  </div>
</template>

<script>
import Child from './Child'

export default {
  data () {
    return {
      val: 'hello'
    }
  },
  components: { 
    Child
  }
}
</script>
```

子组件：

```vue
<template>
  <input :value="val" @input="$emit('input', $event.target.value)" />
</template>

<script>
export default {
  props: {
    val: String
  }
}
</script>
```

父组件可以直接用 `v-model` 优化：（`v-model` 虽然是语法糖，但是在单个 input 组件和自定义组件中，用法还是有区别的，可以对比下 @input 部分，`@input="val = $event" ` vs `@input="val = $event.target.value"`）

```vue
<template>
  <div>
    <Child v-model="val" />
    {{ val }}
  </div>
</template>
```

代码在这个 [commit](https://github.com/vue-demo-space/v-model/tree/78a60172111a440fc93b6e520ccec100f822c6f3)

## 普通父子组件

v-model 的使用场景有限（主要是 v-model 限制了 props 名为 value，有的时候 value 已经被占用了，而且限制了事件名为 input），如果是普通父子组件呢？

父组件：

```vue
<template>
  <div>
    <Child :value.sync="val" />
    {{ val }}
  </div>
</template>

<script>
import Child from './Child'

export default {
  data () {
    return {
      val: 'hello'
    }
  },
  components: { 
    Child
  }
}
</script>
```

子组件：

```vue
<template>
  <input :value="val" @input="$emit('update:value', $event.target.value)" />
</template>

<script>
export default {
  props: {
    val: String
  }
}
</script>
```

`.sync` 表示该值双向绑定，emit 的格式是一定的，`update:myPropName` 的形式

这样就可以绑定任意值了，而不是像 v-model 一样只能绑定 value

代码在 master 上
