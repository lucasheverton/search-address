<template>
  <div class="container">
    <MyTitle />
    <MyForm />
  </div>
</template>

<script>
import MyForm from './components/MyForm.vue'
import MyTitle from './components/MyTitle.vue'

export default {
  name: 'App',
  components: {
    MyForm,
    MyTitle
  }
}
</script>

<style lang="scss">
html {
  font-family: 'Nunito', sans-serif;
  font-size: 14px;
  text-align: center;
  background-color: #ffffff;
  color: #383B41;
  margin-top: 6.142rem;

  .container {
    max-width: 724px;
    margin: 0 auto;
  }
}
</style>
