<template>
  <div id="stellen-single-view">
    <div v-if="loading" class="d-flex justify-content-center mb-3">
      <b-spinner label="Loading..."></b-spinner>
    </div>
    <div id="stellen-single-view-loaded">
      <b-list-group>
        <b-list-group-item v-for="(value, key) in itemData" :key="key">
          <div class="accordion" role="tablist">
            <b-card no-body class="mb-1" :title="key">
              <b-card-title class="title">
                {{ humanize(key) }}
              </b-card-title>
              <b-card-body>
                <b-card-text>
                  <a
                    v-if="key === 'url'"
                    target="_blank"
                    :href="value"
                    class="alert-link"
                    >{{ value }}
                  </a>
                  <vue-json-pretty
                    v-else-if="key === 'orig_data_csv'"
                    :data="JSON.parse(value)"
                  ></vue-json-pretty>
                  <div v-else-if="typeof value === 'string'">
                    {{ value }}
                  </div>
                  <code v-else-if="typeof value === 'number'">{{ value }}</code>
                  <vue-json-pretty v-else :data="value"></vue-json-pretty>
                </b-card-text>
              </b-card-body>
            </b-card>
          </div>
        </b-list-group-item>
      </b-list-group>
    </div>
  </div>
</template>

<script>
import Default from '@/layouts/default'
export default {
  name: 'Slug',
  components: { Default },
  asyncData({ params }) {
    const slug = params.slug
    return { id: slug }
  },
  data() {
    return {
      id: undefined,
      itemData: {},
      loading: true,
    }
  },
  mounted() {
    this.$axios
      .get('https://mmp.acdh-dev.oeaw.ac.at/api/stelle/?id=' + this.id)
      .then((response) => {
        this.itemData = response.data.results.pop()
        this.loading = false
      })
  },
  methods: {
    humanize(stringValue) {
      return stringValue.replace('_', ' ')
    },
  },
}
</script>
<style scoped>
.title {
  text-transform: capitalize;
}
</style>
