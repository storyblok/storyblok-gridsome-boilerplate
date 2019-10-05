<template>
  <Layout>
    <component
      v-if="story.content.component"
      :key="story.content._uid"
      :blok="story.content"
      :is="story.content.component"
    />
  </Layout>
</template>

<script>
import config from '../../gridsome.config'

const loadStoryblokBridge = function(cb) {
  let sbConfigs = config.plugins.filter((item) => {
    return item.use === 'gridsome-source-storyblok'
  })
  let sbConfig = sbConfigs.length > 0 ? sbConfigs[0] : {}
  let script = document.createElement('script')
  script.type = 'text/javascript'
  script.src = `//app.storyblok.com/f/storyblok-latest.js?t=${sbConfig.options.client.accessToken}`
  script.onload = cb
  document.getElementsByTagName('head')[0].appendChild(script)
}

export default {
  data() {
    return {
      story: {content: {}}
    }
  },
  mounted() {
    loadStoryblokBridge(() => { this.initStoryblokEvents() })
  },
  methods: {
    loadStory() {
      window.storyblok.get({
        slug: window.storyblok.getParam('path'),
        version: 'draft'
      }, (data) => {
        this.story = data.story
      })
    },
    initStoryblokEvents() {
      this.loadStory()

      let sb = window.storyblok

      sb.on(['change', 'published'], (payload) => {
        this.loadStory()
      })

      sb.on('input', (payload) => {
        if (this.story && payload.story.id === this.story.id) {
          payload.story.content = sb.addComments(payload.story.content, payload.story.id)
          this.story = payload.story
        }
      })

      sb.pingEditor(() => {
        if (sb.inEditor) {
          sb.enterEditmode()
        }
      })
    }
  }
}
</script>