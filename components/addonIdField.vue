<template lang="pug">
  input(
    type="text"
    placeholder="Enter project ID"
    :value="value"
    @input="updateValue($event.target.value)"
  )
</template>

<script>
export default {
  name: 'AddonIdField',
  props: {
    value: {
      type: String,
      required: true,
    },
  },
  methods: {
    updateValue(value) {
      this.$emit('input', value)
    },
  },
}
</script>
<style lang="stylus" scoped>
input
  height 50px
  border 1px solid #cbc8d0
  padding 5px 10px
  outline 0
  width 100%
  box-shadow 0 4px 6px -1px rgba(0,0,0,.1), 0 2px 4px -1px rgba(0,0,0,.06)
</style>
