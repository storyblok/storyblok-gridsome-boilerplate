<template>
  <div>
    <div v-html="richtext"></div>
  </div>
</template>

<script>
export default {
  props: {
    blok: {
      type: Object,
      default: () => ({})
    }
  },
  computed: {
    content () {
      return this.blok.richtext || { content: [] }
    },
    richtext() {
      return this.$storyapi.richTextResolver.render(this.content)
    }
  }
}
</script>
