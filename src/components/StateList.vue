<template>
  <select v-model="places">
      <option v-for="place in places" :key="place.id">
          {{ place.name }}
      </option>
  </select>
</template>

<script>
  export default {
    data() {
      return {
        places: [
          { name: "", sTax: 100 }
        ]
      }
    }
  };
</script>

<style lang="scss" scoped>
</style>