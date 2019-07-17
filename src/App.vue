<template>
  <div id="app">
    <div class="speed">{{ bpm }}</div>
    <div class="bottom">
      <input class="range" type="range" min="20" max="200" value="60" @input="setBPM">
      <div class="play-button" @click="toggleClick">
        <img v-show="active" class="play-button__icon" src="./assets/pause.svg" />
        <img v-show="!active" class="play-button__icon play-button__icon--play" src="./assets/play.svg" />
      </div>
    </div>
    <audio src="/click.mp3" ref="click" type="audio/mpeg"></audio>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      active: false,
      bpm: 60,
      interval: null
    }
  },
  methods: {
    toggleClick () {
      this.active = !this.active

      if (this.active) {
        this.interval = window.setInterval(this.playClick, 60000 / this.bpm)
      } else {
        window.clearInterval(this.interval)
        this.interval = null
      }
    },
    setBPM ({ target }) {
      this.bpm = target.value

      if (this.active) {
        window.clearInterval(this.interval)
        this.interval = null

        this.interval = window.setInterval(this.playClick, 60000 / this.bpm)
      }
    },
    playClick () {
      this.$refs.click.play()
    }
  }
}
</script>
<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

.speed
  font-size 50px

.bottom
  display flex
  justify-content center

.range
  width 300px

.play-button
  width 60px
  height 60px
  border-radius 50%
  border 5px black solid
  display flex
  justify-content center
  align-items center
  cursor pointer
  margin-left 30px

.play-button__icon
  width 60%
  height 60%
  margin-left 0

  &--play
    margin-left 10%
</style>
