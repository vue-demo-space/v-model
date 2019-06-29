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