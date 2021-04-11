<template>
  <div id="table-wrapper overflow-auto">
    <h2 id="table-heading" class="mt-3 mb-4">Stellen Liste</h2>
    <b-navbar-nav class="ml-auto">
      <b-form>
        <b-form-group>
          <label for="list-stelle-per-page">Results per Page</label>
          <b-form-input
            id="list-stelle-per-page"
            v-model="perPage"
            size="sm"
            class="mr-sm-2 form-control navbar-search"
            type="number"
            min="1"
            max="25"
          ></b-form-input>
        </b-form-group>
      </b-form>
    </b-navbar-nav>
    <b-form @submit.stop.prevent>
      <b-form-group>
        <label for="search-id">Search ID</label>
        <b-form-input
          id="search-id"
          v-model="searchID"
          size="sm"
          type="number"
          min="1"
        >
        </b-form-input>
      </b-form-group>

      <b-form-group>
        <label for="summary-id">Search summary</label>
        <b-form-input
          id="summary-id"
          v-model="searchsummary"
          size="sm"
          type="text"
        >
        </b-form-input
      ></b-form-group>

      <b-form-group>
        <label for="zitat-id">Search zitat</label>
        <b-form-input id="zitat-id" v-model="searchzitat" size="sm" type="text">
        </b-form-input
      ></b-form-group>

      <b-form-group>
        <label for="translation-id">Search translation</label>
        <b-form-input
          id="translation-id"
          v-model="searchtranslation"
          size="sm"
          type="text"
        >
        </b-form-input
      ></b-form-group>

      <b-form-group>
        <label for="kommentar-id">Search kommentar</label>
        <b-form-input
          id="kommentar-id"
          v-model="searchkommentar"
          size="sm"
          type="text"
        >
        </b-form-input
      ></b-form-group>
    </b-form>

    <b-nav-form>
      <b-pagination
        v-model="currentPage"
        :total-rows="rows"
        :per-page="perPage"
        aria-controls="my-table"
      ></b-pagination>
    </b-nav-form>

    <b-table
      :items="fetchStellen"
      :busy="isBusy"
      :fields="fields"
      :filter-included-fields="['url']"
      :per-page="perPage"
      :current-page="currentPage"
      :api-url="apiUrl"
    >
      <template #table-busy>
        <div class="text-center text-danger my-2">
          <b-spinner class="align-middle"></b-spinner>
          <strong>Loading...</strong>
        </div>
      </template>
      <template #cell(url)="data">
        <nuxt-link :to="'/app/stelle/' + getId(data.item)"
          >Show "Stelle" {{ getId(data.item) }}</nuxt-link
        >
      </template>
    </b-table>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      maxStringLength: 40,
      requests: 0,
      searchID: '',
      currentPage: 1,
      rows: undefined,
      perPage: 20,
      isBusy: false,
      searchsummary: '',
      searchzitat: '',
      searchtranslation: '',
      searchkommentar: '',
      fields: [
        { key: 'url', label: 'Detail view' },
        { key: 'display_label', formatter: this.shorten },
        { key: 'summary', formatter: this.shorten },
        { key: 'zitat', formatter: this.shorten },
        { key: 'translation', formatter: this.shorten },
        { key: 'kommentar', formatter: this.shorten },
        { key: 'text.start_date', label: 'from' },
        { key: 'text.end_date', label: 'to' },
      ],
    }
  },

  computed: {
    apiUrl() {
      const url = new URL('https://mmp.acdh-dev.oeaw.ac.at/api/stelle/')
      /**
       * Apparently the Api limits and offsets not on the result, but before filtering. So filtering for an item on page 2
       * will return nothing, while on page 1
       */
      if (
        this.searchID ||
        this.searchsummary ||
        this.searchzitat ||
        this.searchtranslation ||
        this.searchkommentar
      ) {
        url.searchParams.append('id', this.searchID)
        url.searchParams.append('summary', this.searchsummary)
        url.searchParams.append('zitat', this.searchzitat)
        url.searchParams.append('translation', this.searchtranslation)
        url.searchParams.append('kommentar', this.searchkommentar)
      } else {
        const offset = (this.currentPage - 1) * this.perPage
        url.searchParams.append('offset', offset.toString())
        url.searchParams.append('limit', this.perPage)
      }
      return url.toString()
    },
  },

  methods: {
    fetchStellen(_context) {
      this.isBusy = true

      return this.$axios
        .get(_context.apiUrl)
        .then((response) => {
          this.isBusy = false
          this.rows = response.data.count
          this.requests++
          return response.data.results || []
        })
        .catch((reason) => {
          // eslint-disable-next-line no-console
          console.log(reason) // TODO
        })
    },
    getId(row) {
      return row.url.match(/\d+/).pop()
    },
    shorten(string) {
      const ellipsis = ' […]'
      const lengthInclusiveEllipsis = this.maxStringLength + ellipsis.length
      let result
      if (string.length < lengthInclusiveEllipsis) {
        result = string
      } else {
        result = string.substring(0, this.maxStringLength) + ellipsis
      }
      return result
    },
  },
}
</script>

<style scoped>
label {
  text-transform: capitalize;
}
</style>
