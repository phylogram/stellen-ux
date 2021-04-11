<template>
  <div id="dynamic-content" class="mt-3">
    <nuxt-content :document="content" />
  </div>
</template>

<script>
export default {
  name: 'feedback',
  async asyncData({ $content }) {
    const content = await $content('feedback').fetch()
    return {
      content,
    }
  },
}
</script>
