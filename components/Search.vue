<template>
  <b-nav-form>
    <b-form-input
      id="main-search"
      v-model="search"
      size="sm"
      class="mr-sm-2 form-control navbar-search"
      placeholder="Search Content"
      type="text"
      @submit.stop.prevent
      @input="getResults()"
    ></b-form-input>
    <b-popover target="main-search" :show="searchOpen">
      <b-list-group v-if="result.length > 0">
        <b-list-group-item v-for="(article, key) in result" :key="key">
          <b-link :to="article.path">{{ article.title }}</b-link>
        </b-list-group-item>
      </b-list-group>
      <div v-else>Nothing found …</div>
      <b-button @click="search = ''" class="mt-2">Close</b-button>
    </b-popover>
  </b-nav-form>
</template>

<script>
export default {
  name: 'Search',
  data() {
    return {
      search: '',
      result: [],
    }
  },
  computed: {
    searchOpen() {
      return this.search !== ''
    },
  },
  methods: {
    async getResults() {
      this.result = await this.$content('').search(this.search).fetch()
    },
  },
}
</script>
