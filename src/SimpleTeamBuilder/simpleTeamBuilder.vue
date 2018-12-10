<template>
  <div class="simple-team-builder">
    <div class="simple-team-overlay">

      <div class="row">
        <div class="cards-left col-md-3">
          <div class="card">
            <div class="card-body">
              <div class="card-title">
                Top Lane
              </div>
              <div class="btn lane-option" :class="setOptionButtonClass('topLane', option)" @click="optionSelected('topLane', option)" v-for="(option, index) in championTags">
                {{option}}
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
            </div>
          </div>
        </div>
        <div class="main-section-right col-md-8 col-md-offset-1">
          <h1>Create Your Ideal Team Composition</h1>
          <h3>Tag roles in each position based on what you need.</h3>

          <div class="selected-champions-section">
            <div v-show="currentComposition.topLane.champions.length > 0" class="row lane-selected-champions">
              <!-- <img src="/static/images/topLaneIcon.png"> -->
              <img class="lane-icon" src="https://firebasestorage.googleapis.com/v0/b/composition-483b1.appspot.com/o/topLaneIcon.png?alt=media&token=33db9277-47dd-44b7-a0f0-116739e9f465">
              <ChampionDisplayComponent class="championDisplayComponent" v-for="champion in currentComposition.topLane.champions" :championObj="getChampionObjFromName(champion)" :key="champion" :imageStyles="'height:50px;width:50px;margin:5px;'" :hideChampionName="true"></ChampionDisplayComponent>
            </div>
            <div v-show="currentComposition.jungle.champions.length > 0" class="row lane-selected-champions">
              <!-- <img src="/static/images/jungleIcon.png"> -->
              <img class="lane-icon" src="https://firebasestorage.googleapis.com/v0/b/composition-483b1.appspot.com/o/jungleIcon.png?alt=media&token=6a450eca-261b-470b-b7c4-ff54bc52a45e">
              <ChampionDisplayComponent class="championDisplayComponent" v-for="champion in currentComposition.jungle.champions" :championObj="getChampionObjFromName(champion)" :key="champion" :imageStyles="'height:50px;width:50px;margin:5px;'" :hideChampionName="true"></ChampionDisplayComponent>
            </div>
            <div v-show="currentComposition.midLane.champions.length > 0" class="row lane-selected-champions">
              <!-- <img src="/static/images/midLaneIcon.png"> -->
              <img class="lane-icon" src="https://firebasestorage.googleapis.com/v0/b/composition-483b1.appspot.com/o/midLaneIcon.png?alt=media&token=baef3fad-4dc7-4365-9987-198610b8737f">
              <ChampionDisplayComponent class="championDisplayComponent" v-for="champion in currentComposition.midLane.champions" :championObj="getChampionObjFromName(champion)" :key="champion" :imageStyles="'height:50px;width:50px;margin:5px;'" :hideChampionName="true"></ChampionDisplayComponent>
            </div>
            <div v-show="currentComposition.botLane.champions.length > 0" class="row lane-selected-champions">
              <!-- <img src="/static/images/botLaneIcon.png"> -->
              <img class="lane-icon" src="https://firebasestorage.googleapis.com/v0/b/composition-483b1.appspot.com/o/botLaneIcon.png?alt=media&token=5eadc0f0-2d41-46e2-9d1e-6754f46f8f26">
              <ChampionDisplayComponent class="championDisplayComponent" v-for="champion in currentComposition.botLane.champions" :championObj="getChampionObjFromName(champion)" :key="champion" :imageStyles="'height:50px;width:50px;margin:5px;'" :hideChampionName="true"></ChampionDisplayComponent>
            </div>
            <div v-show="currentComposition.support.champions.length > 0" class="row lane-selected-champions">
              <!-- <img src="/static/images/supportIcon.png"> -->
              <img class="lane-icon" src="https://firebasestorage.googleapis.com/v0/b/composition-483b1.appspot.com/o/supportIcon.png?alt=media&token=77603fb2-36d2-4ee8-be14-044841e1a117">
              <ChampionDisplayComponent class="championDisplayComponent" v-for="champion in currentComposition.support.champions" :championObj="getChampionObjFromName(champion)" :key="champion" :imageStyles="'height:50px;width:50px;margin:5px;'" :hideChampionName="true"></ChampionDisplayComponent>
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
        apiLink: `https://ddragon.leagueoflegends.com/cdn/8.14.1/data/en_US/champion.json`,
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
    margin-top: 25px;
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
    position: relative;
    top: 8px;
  }

  // TODO: This is definitely not going to be mobile responsive
  .cards-left {
    max-width: 400px;
    height: 100%;
    margin-left: 15px;
  }

  .lane-icon {
    height: 75px;
    width: 75px;
  }

  .selected-champions-section {
    margin-top: 25px;
    margin-left: 25px;
  }

  .lane-selected-champions {
    margin-bottom: 10px;
  }
</style>

<!-- Add global style to body to make the background color appear everywhere  -->
<style>
  body {
    background-color: #172A3A;
  }
</style>
