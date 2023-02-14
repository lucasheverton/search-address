<template>
  <h1 v-text="title.toUpperCase()"></h1>
</template>

<script>
export default {
  name: "MyTitle",

  data() {
    return {
      title: "Consulta Endereço"
    }
  }
}
</script>

<style lang="scss">
h1 {
  margin: 0;
  padding-bottom: 32px;
  font-size: 32px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
}
</style>