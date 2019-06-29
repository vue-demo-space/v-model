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