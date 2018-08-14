<template>
  <div id="app">
    {{ apiChampionData }}
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      //TODO: Add these to champion list
      apiLink: `http://ddragon.leagueoflegends.com/cdn/8.14.1/data/en_US/champion.json`,
      apiJSON: {api: {}},
      championsList: [],
    }
  },
  mounted() {
    this.axios.get(this.apiLink).then(response => (this.handleAPICall(response)))
  },
  methods: {
    handleAPICall(response) {
      this.apiJSON.api = response
    },
  },
  computed: {
    apiChampionData() {
      return this.apiJSON.api && this.apiJSON.api.data &&
            this.apiJSON.api.data && this.apiJSON.api.data.data
    },
  }
}
</script>

<style lang="scss"
</style>
