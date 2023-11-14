<template>
  <div class="dice-user-value">
    <span class="value">{{ formatUserValue }}</span>
    <span class="reversed">{{ formatReversed }}</span>
  </div>
</template>

<script>
export default {
  name: 'DiceUserValue',
  props: {
    userValue: {
      type: Number,
      required: true
    },
    reversed: {
      type: Boolean,
      required: true
    }
  },
  computed: {
    // Форматирует userValue для вывода на экран
    formatUserValue () {
      return parseFloat(this.userValue).toFixed(2)
    },
    // Форматирует reversed для вывода на экран
    formatReversed () {
      return this.reversed ? ' <' : ' >'
    }
  }
}
</script>

<style>
.dice-user-value {
  margin-top: 53px;
  font-size: 28px;
  line-height: 1;
  color: #5e516d;
}

/* 414px  iPhone 6/7/8 */
@media (min-width: 414px) and (max-width: 812px) {
  .dice-user-value {
    margin-top: 0;
    font-size: 16px;
    line-height: 1.75;
    text-align: center;
  }
}

/* 320px  iPhone 5/SE */
@media (min-width: 320px) and (max-width: 413px) {
  .dice-user-value {
    margin-top: 0;
    font-size: 16px;
    line-height: 1.75;
    text-align: center;
  }
}
</style>
