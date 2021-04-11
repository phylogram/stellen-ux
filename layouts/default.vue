<template>
  <div class="page">
    <Navigation />
    <b-container fluid="sm">
      <b-row>
        <b-col>
          <Nuxt></Nuxt>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
import Navigation from '../components/Navigation'

export default {
  components: { Navigation },
}
</script>
