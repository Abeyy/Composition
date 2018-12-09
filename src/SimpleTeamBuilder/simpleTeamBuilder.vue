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
      }
      },
      optionSelected (lane, option) {
        let rolesArray = this.currentComposition[lane].roles
        let championsArray = this.currentComposition[lane].champions

        if (!rolesArray.includes(option)) {
          if (rolesArray.length == 2) {
            // Remove the first element in roles, then shift the array down.
            rolesArray.shift()
          }
          rolesArray.push(option)
        }

        // If there are two roles, we only want champions that have BOTH roles so clear out the array:
        if (rolesArray && rolesArray.length === 2) {
          championsArray.length = 0
        }

        this.championsList.map((champion) => {
          if (rolesArray && rolesArray.length === 2) {
            // Need to sort both roles and champion tags, then see if the arrays are equal
            // Copy over array using empty .slice()
            let rolesArrayCopy = rolesArray.slice()
            let championTagsCopy = this.apiJSON.championsObj[champion].tags.slice()
            rolesArrayCopy.sort()
            championTagsCopy.sort()
            let equal = rolesArrayCopy.length == championTagsCopy.length && rolesArrayCopy.every((element, index)=> element === championTagsCopy[index]);

            if (equal && !championsArray.includes(champion)) {
              championsArray.push(champion)
            }
          } else if (this.apiJSON.championsObj[champion].tags.includes(option) && !championsArray.includes(champion)) {
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
