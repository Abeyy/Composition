<template>
  <div class="championsList">
    {{ championsList }}
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      //TODO: Add these to champion list
      apiLink: `http://ddragon.leagueoflegends.com/cdn/8.14.1/data/en_US/champion.json`,
      apiJSON: {
        api: {},
        championsObj: {}
      },
      championsList: [],
    }
  },
  mounted() {
    this.axios.get(this.apiLink).then(response => (this.handleAPICall(response)))
  },
  methods: {
    handleAPICall (response) {
      this.apiJSON.api = response
      this.apiJSON.championsObj =
      this.initializeChampionsListFromAPICall(response)
    },
    initializeChampionsListFromAPICall (response) {
      let championDataObj = this.apiJSON.api.data.data
      this.championsList = Object.keys(championDataObj)
    }
  },
  computed: {}
}
</script>

<style lang="scss"
</style>
