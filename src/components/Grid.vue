<template>
  <div class="grid" v-editable="blok">
    <component
      :key="blok._uid"
      v-for="blok in blok.columns"
      :blok="blok"
      :is="blok.component"
    />
  </div>
</template>

<script>
export default {
  props: ['blok']
}
</script>
