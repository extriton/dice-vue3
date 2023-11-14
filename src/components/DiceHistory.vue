<template>
  <div class="dice-history">
    <transition name="list">
      <ul
        :key="gameCounter"
        class="dice-history-list"
      >
        <li
          v-for="item in formatHistory"
          :key="item"
          class="dice-history-item"
          :class="{ lose: item < 0 }"
        >
          {{ formatItem(item) }}
        </li>
      </ul>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'DiceHistory',
  props: {
    history: {
      type: Array,
      required: true
    },
    gameCounter: {
      type: Number,
      required: true
    }
  },
  computed: {
    formatHistory () {
      return this.history.slice(-6).reverse()
    }
  },
  methods: {
    formatItem(value) {
      return Math.abs(value).toFixed(2)
    }
  }
}
</script>

<style>
.dice-history {
  width: 53px;
  max-height: 133px;
  padding-top: 8px;
  overflow: hidden;
  position: relative;
}

.dice-history-list {
  height: 165px;
}

.dice-history-item {
  display: block;
  height: 22px;
  padding: 3px;
  margin-bottom: 10px;
  text-align: center;
  color: #91ff4a;
  font-size: 12px;
  line-height: 16px;
  border-radius: 11.5px;
  background-color: rgba(255, 255, 255, 0.1);
}

.dice-history-item.lose {
  color: #ff6764;
}

.dice-history-item:nth-child(1) { opacity: 1; }
.dice-history-item:nth-child(2) { opacity: 0.7; }
.dice-history-item:nth-child(3) { opacity: 0.3; }
.dice-history-item:nth-child(4) { opacity: 0.1; }
.dice-history-item:nth-child(5) { opacity: 0; }

.list-enter .dice-history-item:nth-child(1) { opacity: 1; }
.list-enter .dice-history-item:nth-child(2) { opacity: 1; }
.list-enter .dice-history-item:nth-child(3) { opacity: 0.7; }
.list-enter .dice-history-item:nth-child(4) { opacity: 0.3; }
.list-enter .dice-history-item:nth-child(5) { opacity: 0.1; }

.list-enter-to .dice-history-item:nth-child(1) { opacity: 1; }
.list-enter-to .dice-history-item:nth-child(2) { opacity: 0.7; }
.list-enter-to .dice-history-item:nth-child(3) { opacity: 0.3; }
.list-enter-to .dice-history-item:nth-child(4) { opacity: 0.1; }
.list-enter-to .dice-history-item:nth-child(5) { opacity: 0; }

.list-enter-active .dice-history-item {
  transition: opacity .25s ease-out 1.3s;
}

/* Transition styles */
.list-enter-active { transition: transform .25s ease-out 1.3s; }
.list-enter { transform: translateY(-32px); }
.list-enter-to { transform: translateY(0); }

/* 414px  iPhone 6/7/8 */
@media (min-width: 414px) and (max-width: 812px) {
  .dice-history {
    width: 360px;
    height: 22px;
    padding-top: 0;
  }

  .dice-history-list {
    width: 450px;
  }

  .dice-history-item {
    display: inline-block;
    width: 60px;
    margin-bottom: 0;
    margin-right: 15px;
  }

  .dice-history-item:nth-child(1) { opacity: 1; }
  .dice-history-item:nth-child(2) { opacity: 0.8; }
  .dice-history-item:nth-child(3) { opacity: 0.6; }
  .dice-history-item:nth-child(4) { opacity: 0.4; }
  .dice-history-item:nth-child(5) { opacity: 0.2;}
  .dice-history-item:nth-child(6) { opacity: 0.1; }

  .list-enter .dice-history-item:nth-child(1) { opacity: 1; }
  .list-enter .dice-history-item:nth-child(2) { opacity: 1; }
  .list-enter .dice-history-item:nth-child(3) { opacity: 0.8; }
  .list-enter .dice-history-item:nth-child(4) { opacity: 0.6; }
  .list-enter .dice-history-item:nth-child(5) { opacity: 0.4; }
  .list-enter .dice-history-item:nth-child(6) { opacity: 0.2; }

  .list-enter-to .dice-history-item:nth-child(1) { opacity: 1; }
  .list-enter-to .dice-history-item:nth-child(2) { opacity: 0.8; }
  .list-enter-to .dice-history-item:nth-child(3) { opacity: 0.6; }
  .list-enter-to .dice-history-item:nth-child(4) { opacity: 0.4; }
  .list-enter-to .dice-history-item:nth-child(5) { opacity: 0.2; }
  .list-enter-to .dice-history-item:nth-child(6) { opacity: 0.1; }

  /* Transition styles */
  .list-enter { transform: translateX(-75px); }
  .list-enter-to { transform: translateX(0); }
}

/* 320px  iPhone 5/SE */
@media (min-width: 320px) and (max-width: 413px) {
  .dice-history {
    width: 265px;
    height: 22px;
    padding-top: 0;
  }

  .dice-history-list {
    width: 330px;
  }

  .dice-history-item {
    display: inline-block;
    width: 45px;
    margin-bottom: 0;
    margin-right: 10px;
  }

  .dice-history-item:nth-child(1) { opacity: 1; }
  .dice-history-item:nth-child(2) { opacity: 0.8; }
  .dice-history-item:nth-child(3) { opacity: 0.6; }
  .dice-history-item:nth-child(4) { opacity: 0.4; }
  .dice-history-item:nth-child(5) { opacity: 0.2;}
  .dice-history-item:nth-child(6) { opacity: 0.1; }

  .list-enter .dice-history-item:nth-child(1) { opacity: 1; }
  .list-enter .dice-history-item:nth-child(2) { opacity: 1; }
  .list-enter .dice-history-item:nth-child(3) { opacity: 0.8; }
  .list-enter .dice-history-item:nth-child(4) { opacity: 0.6; }
  .list-enter .dice-history-item:nth-child(5) { opacity: 0.4; }
  .list-enter .dice-history-item:nth-child(6) { opacity: 0.2; }

  .list-enter-to .dice-history-item:nth-child(1) { opacity: 1; }
  .list-enter-to .dice-history-item:nth-child(2) { opacity: 0.8; }
  .list-enter-to .dice-history-item:nth-child(3) { opacity: 0.6; }
  .list-enter-to .dice-history-item:nth-child(4) { opacity: 0.4; }
  .list-enter-to .dice-history-item:nth-child(5) { opacity: 0.2; }
  .list-enter-to .dice-history-item:nth-child(6) { opacity: 0.1; }

  /* Transition styles */
  .list-enter { transform: translateX(-55px); }
  .list-enter-to { transform: translateX(0); }
}
</style>
