<template>
  <div
    class="dice-reverse"
    :class="{ clicked: clicked }"
    @click="reverse"
    @mousedown.left="clicked = true"
    @touchstart="clicked = true"
    @mouseup.left="clicked = false"
    @touchend="clicked = false"
    @mouseleave="clicked = false"
  >
  </div>
</template>

<script>
export default {
  name: 'DiceReverse',
  data () {
    return {
      clicked: false
    }
  },
  methods: {
    reverse () {
      this.$emit('reverse')
    }
  }
}
</script>

<style>
.dice-reverse {
  width: 34px;
  height: 34px;
  border: 5px solid #291a43;
  border-radius: 100%;
  background: url('@/assets/change.svg') no-repeat #3e3156;
  background-position: 3px 3px;
}
.dice-reverse:hover {
  cursor: pointer;
  background: url('@/assets/change_h.svg') no-repeat #fff;
  background-position: 3px 3px;
}
.dice-reverse.clicked {
  cursor: pointer;
  background: url('@/assets/change_h.svg') no-repeat #ffed2a;
  background-position: 3px 3px;
}
</style>
