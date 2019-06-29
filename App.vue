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