//TODO: pass in champion information and custom styles as props.
<template>
  <div class="championDisplayComponent">
    <div v-if="!hideChampionImage" class="champion-image">
      <img :style="imageStyles" v-bind:src="championImageURL">
    </div>
    <div v-if="!hideChampionName" class="champion-name">
      {{ championObj.name }}, {{ championObj.title}}
    </div>

  </div>
</template>

<script>
  export default {
    props: [ 'championObj', 'hideChampionImage', 'hideChampionName', 'imageStyles' ],
    data() {
      return {
        baseImageUrl: `https://ddragon.leagueoflegends.com/cdn/8.14.1/img/champion/`
      }
    },
    components: {},
    mounted() {},
    computed: {
      championImageURL () {
        return `${this.baseImageUrl}${this.championObj.image.full}`
      }
    },
    methods: {},
    watch: {}
  }
</script>

<style scoped>
</style>
