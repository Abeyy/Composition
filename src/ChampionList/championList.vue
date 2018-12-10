<template>
  <div class="championListComponent">


  <div class="championsList" v-for="(championName, index) in championsList">
    <ChampionDisplay :championObj="getChampionObjFromName(championName)"></ChampionDisplay>
  </div>

  </div>
</template>

<script>
import ChampionDisplay from '../ChampionDisplayComponent/championDisplayComponent.vue'
export default {
  components: {
    ChampionDisplay
  },
  data () {
    return {
      //TODO: Add these to champion list
      apiLink: `https://ddragon.leagueoflegends.com/cdn/8.14.1/data/en_US/champion.json`,
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
    getChampionObjFromName (championName) {
      return this.apiJSON.championsObj[championName]
    },
    handleAPICall (response) {
      this.apiJSON.api = response
      this.apiJSON.championsObj = this.apiJSON.api.data.data
      this.initializeChampionsListFromAPICall(response)
    },
    initializeChampionsListFromAPICall (response) {
      let championDataObj = this.apiJSON.api.data.data
      this.championsList = Object.keys(championDataObj)
    }
  },
  computed: {
    aatrox () {
      let Aatrox = this.championsList[0]
      return this.apiJSON.championsObj.Aatrox
    }
  }
}
</script>

<style lang="scss"
</style>
