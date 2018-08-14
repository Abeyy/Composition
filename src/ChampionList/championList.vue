<template>
  <div id="app">
    {{ Aatrox }}
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

    // this.addChampionsToList()
  },
  methods: {
    handleAPICall(response) {
      this.apiJSON.api = response
      // this.apiJSON = this.apiJSON.api && this.apiJSON.api.data
    },
    addChampionsToList() {
    }
  },
  computed: {
    Aatrox() {
      return this.apiJSON.api && this.apiJSON.api.data &&
            this.apiJSON.api.data && this.apiJSON.api.data.data.Aatrox
    }
  }
}
</script>

<style lang="scss"
</style>
