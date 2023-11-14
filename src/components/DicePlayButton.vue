<template>
  <div
    class="dice-play-button"
    :class="{ clicked: clicked }"
    @click="play"
    @mousedown.left="clicked = true"
    @touchstart="clicked = true"
    @mouseup.left="clicked = false"
    @touchend="clicked = false"
    @mouseleave="clicked = false"
  >
    Играть
  </div>
</template>

<script>
export default {
  name: 'DicePlayButton',
  data () {
    return {
      clicked: false
    }
  },
  methods: {
    // Эмит сообщения о начале розыгрыша
    play () {
      this.$emit('dicePlay')
    }
  }
}
</script>

<style>
.dice-play-button {
  width: 136px;
  height: 50px;
  padding: 13px 14px 16px 15px;
  border-radius: 5px;
  box-shadow: inset 0 -3px 0 0 rgba(0, 0, 0, 0.24);
  background-image: linear-gradient(to bottom, #ffed2a 5%, #ffdc2a 94%);
  margin: 0 auto;
  font-size: 18px;
  font-weight: bold;
  line-height: 21px;
  text-align: center;
  color: #3f2652;
}

.dice-play-button:hover {
  cursor: pointer;
  box-shadow: inset 0 -3px 0 0 rgba(0, 0, 0, 0.24);
  background-image: linear-gradient(to bottom, #ffcb00 5%, #ffaf00 94%);
}

.dice-play-button.clicked {
  box-shadow: inset 0 3px 0 0 rgba(0, 0, 0, 0.24);
  background-color: #ffbb00;
}

/* 414px  iPhone 6/7/8 */
@media (min-width: 414px) and (max-width: 812px) {
  .dice-play-button {
    width: 100%;
  }
}

/* 320px  iPhone 5/SE */
@media (min-width: 320px) and (max-width: 413px) {
  .dice-play-button {
    width: 100%;
  }
}
</style>
