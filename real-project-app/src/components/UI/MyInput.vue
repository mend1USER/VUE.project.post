<template>
  <input :value="modalValue" @input="updateInput" class="input" type="text">
</template>

<script>
export default {
  name:'my-input',
  props: {
    modalValue: [String, Number]
  },
  methods: {
    updateInput(event) {
      this.$emit('update:modalValue', event.target.value)
    }
  }
}
</script>

<style scoped>
.input{
  width: 100%;
  border: 1px solid teal;
  padding: 10px 15px;
  margin-top: 15px;
}
</style>