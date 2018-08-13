<template>
  <div id="app">
    json: {{apiJSON}}
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      apiLink: `http://ddragon.leagueoflegends.com/cdn/8.14.1/data/en_US/champion.json`,
      apiJSON: ''
    }
  },
  mounted() {
    this.axios.get(this.apiLink).then(response => (this.apiJSON = response))
  }
}
</script>

<style lang="scss"
</style>
