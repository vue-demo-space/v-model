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