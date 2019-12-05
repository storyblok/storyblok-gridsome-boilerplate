<template>
  <div>
    <g-image :src="imageURL"></g-image>
  </div>
</template>

<script>
export default {
  props: ['blok'],
  computed: {
    imageURL () {
      if (typeof this.blok.image === 'string') {
        return this.blok.image
      }

      const path = this.blok.image.path
      return require('!!assets-loader?width=800&quality=100&fit=inside!~/' + path)
    }
  }
}
</script>

<style scoped>
img {
  max-width: 100%;
}
</style>