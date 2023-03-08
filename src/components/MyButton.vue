<template>
  <button
    type="submit"
    v-text="name"
    @click="clearFormData">
  </button>
</template>

<script>
export default {
  name: "MyButton",
  props: {
    name: {
      required: true,
      typeOf: String
    }
  },

  methods: {
    clearFormData() {
      this.$emit('clear-all-data')
    }
  }
}
</script>

<style lang="scss">
button {
  margin-top: 32px;
  padding: 0;
  width: 276px;
  height: 44px;
  background: #383b41;
  color: #FFFFFF;
  border-radius: 10px;
  border: none;
  outline: 0;
  cursor: pointer;

  &:hover {
    background-color: rgba(56, 59, 65, .80);
    transition: ease-in-out .4s;
  }
}
</style>