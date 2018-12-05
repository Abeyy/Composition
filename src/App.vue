<template>
  <div id="app">
    <!-- <ChampionList></ChampionList> -->
    <SimpleTeamBuilder></SimpleTeamBuilder>
  </div>
</template>

<script>
import ChampionList from './ChampionList/championList.vue'
import SimpleTeamBuilder from './SimpleTeamBuilder/simpleTeamBuilder.vue'

export default {
  name: 'app',
  components: {
    ChampionList,
    SimpleTeamBuilder
  },
  data () {
    return {}
  }
}
</script>

<style lang="scss"
</style>
