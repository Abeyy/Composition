//TODO: pass in champion information and custom styles as props.
<template>
  <div class="championDisplayComponent">
    <div class="champion-image">
      <img v-bind:src="championImageURL">
    </div>
    <div class="champion-name">
      {{ championObj.name }}, {{ championObj.title}}
    </div>

  </div>
</template>

<script>
  export default {
    props: [ 'championObj' ],
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
