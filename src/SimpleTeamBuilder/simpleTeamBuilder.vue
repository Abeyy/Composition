<template>
  <div>
    <h1> Choose Your Team Composition: </h1>
    <div class="role-selection-section">
      <button class="btn btn-primary" @click="showLaneOptions('topLane')"> Top Lane </button>
      <button class="btn btn-primary" @click="showLaneOptions('jungle')"> Jungle </button>
      <button class="btn btn-primary" @click="showLaneOptions('midLane')"> Mid Lane </button>
      <button class="btn btn-primary" @click="showLaneOptions('botLane')"> Bot Lane </button>
      <button class="btn btn-primary" @click="showLaneOptions('support')"> Support </button>
    </div>

    <div class="lane-options">
      <div class="top-lane-options lane-option" @click="optionSelected('topLane', option)" v-show="showTopLaneOptions" v-for="option in topLaneOptions">
        {{option}}
      </div>
    </div>

    <div class="suggested-lane-champion" v-for="lane in lanes">
      <div class="suggested-top-laners" v-show="currentComposition.topLane.champions.length > 0">
       Hi Hello  {{currentComposition.topLane.champions}}
      </div>
    </div>

  </div>
</template>

<script>
  export default {
    props: [],
    data() {
      return {
        apiLink: `http://ddragon.leagueoflegends.com/cdn/8.14.1/data/en_US/champion.json`,
        apiJSON: {
          api: {},
          championsObj: {}
        },
        championsList: [],
        showTopLaneOptions: false,
        showJungleOptions: false,
        showMidLaneOptions: false,
        showBottomLaneOptions: false,
        showSupportOptions: false,
        lanes: ['topLane', 'jungle', 'midLane', 'botLane', 'support'],
        topLaneOptions: ['Tank', 'Fighter', 'Mage', 'Assassin'],
        currentComposition: {
          topLane: {
            roles: [],
            champions: []
          },
          jungle: {
            roles: [],
            champions: []
          },
          midLane: {
            roles: [],
            champions: []
          },
          botLane: {
            roles: [],
            champions: []
          },
          support: {
            roles: [],
            champions: []
          }
        }
      }
    },
    components: {},
    mounted() {
      this.axios.get(this.apiLink).then(response => (this.handleAPICall(response)))
    },
    computed: {},
    methods: {
      handleAPICall (response) {
        this.apiJSON.api = response
        this.apiJSON.championsObj = this.apiJSON.api.data.data
        this.initializeChampionsListFromAPICall(response)
      },
      initializeChampionsListFromAPICall (response) {
        let championDataObj = this.apiJSON.api.data.data
        this.championsList = Object.keys(championDataObj)
      },
      showLaneOptions (lane) {
        switch (lane) {
          case 'topLane':
            this.showTopLaneOptions = true
            break;
          case 'jungle':
            this.showJungleOptions = true
            break;
          case 'midLane':
            this.showMidLaneOptions = true
            break;
          case 'bottomLane':
            this.showBottomLaneOptions = true
            break;
          case 'support':
            this.showSupportOptions = true
            break;
        }
      },
      optionSelected (lane, option) {
        let rolesArray = this.currentComposition[lane].roles
        let championsArray = this.currentComposition[lane].champions

        if (!rolesArray.includes(option)) {
          this.currentComposition[lane].roles.push(option)
        }

        this.championsList.map((champion) => {
          if (this.apiJSON.championsObj[champion].tags.includes(option) && !championsArray.includes(champion)) {
            championsArray.push(champion)
          }
        })

      }
    },
    watch: {}
  }
</script>

<style scoped>
  .lane-option {
    cursor: pointer;
  }


</style>
