<template>
  <pre v-if="displayable">
    {{ dataApi }}
  </pre>
</template>

<script>
export default {
  data() {
    return {}
  },

  props: {
    dataApi: {
      typeof: Object,
      required: true
    },

    displayable: {
      typeof: Boolean,
    }
  }
}
</script>