<template>
  <div class="simple-team-builder">
    <div class="opacity-overlay">
    <div class="simple-team-overlay">
      <h1> Choose Your Team Composition: </h1>

      <div class="row">
        <div class="col-xs-12 col-md-3 offset-md-1">
          <div class="card">
            <div class="card-body">
              <div class="card-title">
                Top Lane
              </div>
              <div class="btn btn-info top-lane-options lane-option" :class="[currentComposition.topLane.roles.includes(option) ? '' : 'unselected-option']" @click="optionSelected('topLane', option)" v-for="option in topLaneOptions">
                {{option}}
              </div>

              <div class="suggested-top-laners" v-show="currentComposition.topLane.champions.length > 0">
                <ChampionDisplayComponent class="championDisplayComponent" v-for="champion in currentComposition.topLane.champions" :championObj="getChampionObjFromName(champion)" :key="champion" :imageStyles="'height:50px;width:50px;margin:5px;'" :hideChampionName="true"></ChampionDisplayComponent>
              </div>
            </div>
          </div>
        </div>

        <div class="col-xs-12 col-md-3">
          <div class="card">
            <div class="card-body">
              <div class="card-title">
                Jungle
              </div>
            </div>
          </div>
        </div>

        <div class="col-xs-12 col-md-3">
          <div class="card">
            <div class="card-body">
              <div class="card-title">
                Mid Lane
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="row">
        <div class="col-xs-12 col-md-3 offset-2">
          <div class="card">
            <div class="card-body">
              <div class="card-title">
                Bot Lane
              </div>
            </div>
          </div>
        </div>

        <div class="col-xs-12 col-md-3">
          <div class="card">
            <div class="card-body">
              <div class="card-title">
                Support
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- <div class="role-selection-section">
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
      </div> -->


    </div>
    </div>
  </div>
</template>

<script>
  import ChampionDisplayComponent from '../ChampionDisplayComponent/championDisplayComponent.vue'
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
    components: {
      ChampionDisplayComponent
    },
    mounted() {
      this.axios.get(this.apiLink).then(response => (this.handleAPICall(response)))
    },
    computed: {},
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
          if (rolesArray.length == 2) {
            rolesArray[0] = option
          } else {
            this.currentComposition[lane].roles.push(option)
          }
        }

        console.log(rolesArray)

        this.championsArray = []

        this.championsList.map((champion) => {
          if (this.apiJSON.championsObj[champion].tags.includes(option) && !championsArray.includes(champion)) {
            championsArray.push(champion)
          }
        })

        this.sortChampionListByRole(lane, option)
      },
      sortChampionListByRole (lane, option) {
        let rolesArray = this.currentComposition[lane].roles
        let championsArray = this.currentComposition[lane].champions

        championsArray.sort((a,b) => {
          let aRoles = this.apiJSON.championsObj[a].tags
          let bRoles = this.apiJSON.championsObj[b].tags

          let aSimilar = aRoles.filter(value => -1 !== rolesArray.indexOf(value));
          let bSimilar = bRoles.filter(value => -1 !== rolesArray.indexOf(value));
          return bSimilar.length - aSimilar.length
        })
      }
    },
    watch: {}
  }
</script>

<style scoped lang="less">
  @color1: #74B3CE;
  @color2: #508991;
  @color3: #172A3A;
  @color4: #004346;
  @color5: #078965;

  .lane-option {
    cursor: pointer;
  }

  .unselected-option {
    opacity: 0.5;
  }

  .simple-team-builder {
    background-color: @color3;
    text-align: center;
    position:relative;
    height: 100vh;

    .card {
      margin-bottom: 10px;

      .card-body {
        padding: 10px;
      }

      .lane-option {
        margin: 1px;
      }
    }
  }

  .simple-team-overlay {
    z-index: 2;
    color: @color1;
  }

  .role-selection-section {
    button {
      color: @color5;
      background-color: @color4;
      border-color: @color4;
    }
  }

  .championDisplayComponent {
    display: inline-block;
  }

</style>
