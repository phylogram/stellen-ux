<template>
  <div id="dynamic-content" class="mt-3">
    <nuxt-content :document="content" />
  </div>
</template>

<script>
export default {
  name: 'about',
  async asyncData({ $content }) {
    const content = await $content('about').fetch()
    return {
      content,
    }
  },
}
</script>
