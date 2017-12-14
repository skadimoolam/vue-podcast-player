<template>
  <audio ref="player" controls></audio>
</template>

<script>
export default {
  name: 'player',

  props: {
    track: {
      type: String,
      default: "",
      required: true
    }
  },

  watch: {
    track(val, oldVal) {
      this.$refs.player.src = val
      this.$refs.player.play()
    }
  }
}
</script>