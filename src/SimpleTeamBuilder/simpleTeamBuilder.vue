<template>
  <div class="simple-team-builder">
    <div class="opacity-overlay">
    <div class="simple-team-overlay">
      <h1> Choose Your Team Composition:</h1>

      <div class="cards-left">
        <div class="card">
          <div class="card-body">
            <div class="card-title">
              Top Lane
            </div>
            <div class="btn lane-option" :class="setOptionButtonClass('topLane', option)" @click="optionSelected('topLane', option)" v-for="(option, index) in championTags">
              {{option}}
            </div>

            <div class="suggested-top-laners" v-show="currentComposition.topLane.champions.length > 0">
              <ChampionDisplayComponent class="championDisplayComponent" v-for="champion in currentComposition.topLane.champions" :championObj="getChampionObjFromName(champion)" :key="champion" :imageStyles="'height:50px;width:50px;margin:5px;'" :hideChampionName="true"></ChampionDisplayComponent>
            </div>
          </div>
        </div>

        <div class="card">
          <div class="card-body">
            <div class="card-title">
              Jungle
            </div>
            <div class="btn lane-option" :class="setOptionButtonClass('jungle', option)" @click="optionSelected('jungle', option)" v-for="option in championTags">
              {{option}}
            </div>

            <div class="suggested-top-laners" v-show="currentComposition.jungle.champions.length > 0">
              <ChampionDisplayComponent class="championDisplayComponent" v-for="champion in currentComposition.jungle.champions" :championObj="getChampionObjFromName(champion)" :key="champion" :imageStyles="'height:50px;width:50px;margin:5px;'" :hideChampionName="true"></ChampionDisplayComponent>
            </div>
          </div>
        </div>

        <div class="card">
          <div class="card-body">
            <div class="card-title">
              Mid Lane
            </div>
            <div class="btn lane-option" :class="setOptionButtonClass('midLane', option)" @click="optionSelected('midLane', option)" v-for="option in championTags">
              {{option}}
            </div>

            <div class="suggested-top-laners" v-show="currentComposition.midLane.champions.length > 0">
              <ChampionDisplayComponent class="championDisplayComponent" v-for="champion in currentComposition.midLane.champions" :championObj="getChampionObjFromName(champion)" :key="champion" :imageStyles="'height:50px;width:50px;margin:5px;'" :hideChampionName="true"></ChampionDisplayComponent>
            </div>
          </div>
        </div>

        <div class="card">
          <div class="card-body">
            <div class="card-title">
              Bot Lane
            </div>
            <div class="btn lane-option" :class="setOptionButtonClass('botLane', option)" @click="optionSelected('botLane', option)" v-for="option in championTags">
              {{option}}
            </div>

            <div class="suggested-top-laners" v-show="currentComposition.botLane.champions.length > 0">
              <ChampionDisplayComponent class="championDisplayComponent" v-for="champion in currentComposition.botLane.champions" :championObj="getChampionObjFromName(champion)" :key="champion" :imageStyles="'height:50px;width:50px;margin:5px;'" :hideChampionName="true"></ChampionDisplayComponent>
            </div>
          </div>
        </div>

        <div class="card">
          <div class="card-body">
            <div class="card-title">
              Support
            </div>
            <div class="btn lane-option" :class="setOptionButtonClass('support', option)" @click="optionSelected('support', option)" v-for="option in championTags">
              {{option}}
            </div>

            <div class="suggested-top-laners" v-show="currentComposition.support.champions.length > 0">
              <ChampionDisplayComponent class="championDisplayComponent" v-for="champion in currentComposition.support.champions" :championObj="getChampionObjFromName(champion)" :key="champion" :imageStyles="'height:50px;width:50px;margin:5px;'" :hideChampionName="true"></ChampionDisplayComponent>
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
        lanes: ['topLane', 'jungle', 'midLane', 'botLane', 'support'],
        topLaneOptions: ['Tank', 'Fighter', 'Mage', 'Assassin'],
        //TODO: Add subclasses: https://na.leagueoflegends.com/en/news/game-updates/gameplay/taking-another-look-subclasses
        championTags: ['Tank', 'Fighter', 'Mage', 'Assassin', 'Marksman', 'Support'],
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
      setOptionButtonClass (lane, option) {
        let btnClassName
        let isSelectedOption
        switch (option) {
          case "Mage":
            btnClassName = 'btn-outline-primary'
            break
          case "Tank":
            btnClassName = 'btn-outline-secondary'
            break
          case "Marksman":
            btnClassName = 'btn-outline-success'
            break
          case "Assassin":
            btnClassName = 'btn-outline-danger'
            break
          case "Fighter":
            btnClassName = 'btn-outline-warning'
            break
          case "Support":
            btnClassName = 'btn-outline-info'
            break
        }

        this.currentComposition[lane].roles.includes(option) ? btnClassName = btnClassName.replace('outline-','') : isSelectedOption = 'unselected-option'


        return [btnClassName, isSelectedOption]
      },
      deselectOptionAndRemoveChampions (lane, option) {
        let rolesArray = this.currentComposition[lane].roles
        let championsArray = this.currentComposition[lane].champions

        rolesArray.splice(rolesArray.indexOf(option), 1)

        // If there is only one role remaining, clear out the list so we can add solely that role later
        // We need to do this here cause we dont want to keep clearing out the list in the loop.
        if (rolesArray.length === 1) {
          championsArray.length = 0
        }

        // We have to map on championsList instead of championsArray because championsArray gets mutated in the loop, which caused weird behavior
        this.championsList.map((champion) => {
          // If there are no roles left, deselect everything.
          if (rolesArray.length === 0) {
            if (this.apiJSON.championsObj[champion].tags.includes(option) && championsArray.includes(champion)) {
              championsArray.splice(champion, 1)
            }
            // If there is one role remaining, clear out the list and only add that one role.
          } else if (rolesArray.length === 1) {
            if (this.apiJSON.championsObj[champion].tags.includes(rolesArray[0]) && !championsArray.includes(champion)) {
              championsArray.push(champion)
            }
          }
        })
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
        } else {
          return this.deselectOptionAndRemoveChampions(lane, option)
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

  // TODO: This is definitely not going to be mobile responsive
  .cards-left {
    max-width: 400px;
    margin-left: 10px;
  }
</style>

<!-- Add global style to body to make the background color appear everywhere  -->
<style>
  body {
    background-color: #172A3A;
  }
</style>
