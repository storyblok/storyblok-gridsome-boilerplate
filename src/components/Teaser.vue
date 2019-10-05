<template>
  <div class="teaser"
       v-editable="blok">
    {{ blok.headline }}
    <richtext :text="blok.richtext">
    </richtext>
  </div>
</template>

<script>
export default {
  props: ['blok']
}
</script>
