<template>
  <div class="dice-honeycomb">
    <transition
      name="odometr"
      @before-enter="beforeEnter"
    >
      <ul
        ref="digits"
        :key="gameCounter"
        class="dice-honeycomb-digits"
        :class="classObj"
      >
        <li
          v-for="(item, index) in outArray"
          :key="'d' + index"
          class="dice-honeycomb-digit"
        >
          {{ item }}
        </li>
      </ul>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'DiceHoneycomb',
  props: {
    digit: {
      type: Number,
      required: true
    },
    delay: {
      type: Number,
      required: true
    },
    gameCounter: {
      type: Number,
      required: true
    }
  },
  data () {
    return {
      digitsArray: [this.digit],
      animated: false
    }
  },
  computed: {
    outArray () {
      return this.digitsArray
    },
    classObj () {
      const styleName = 'delay-' + this.delay
      return {
        [styleName]: true,
        animated: this.animated
      }
    }
  },
  methods: {
    beforeEnter () {
      // Создаём массив цифр
      const digitsArray = []
      const oldDigit = this.digitsArray[this.digitsArray.length - 1]
      for (let i = oldDigit; i >= 0; i--) {
        digitsArray.push(i)
      }
      for (let i = 9; i >= 0; i--) {
        digitsArray.push(i)
      }
      for (let i = 9; i >= parseInt(this.digit); i--) {
        digitsArray.push(i)
      }
      this.digitsArray = digitsArray

      // Запускаем отолженную анимацию
      setTimeout(() => {
        this.animated = true
        this.digitsArray = this.digitsArray.slice(-1)
        setTimeout(() => {
          this.animated = false
        }, 200)
      }, 1000 + this.delay)
    }
  }
}
</script>

<style>
.dice-honeycomb {
  height: 105px;
  margin-top: 15px;
  overflow: hidden;
}

.dice-honeycomb-digit {
  position: relative;
  font-size: 65px;
  font-weight: bold;
  color: #fff;
  line-height: 100px;
  text-align: center;
}

.odometr-enter-active {
  transition: transform 1s cubic-bezier(0.29, 0.01, .02, 0.99);
}
.odometr-enter-active.delay-100 { transition-delay: .1s }
.odometr-enter-active.delay-200 { transition-delay: .2s }
.odometr-enter-active.delay-300 { transition-delay: .3s }

.odometr-enter { transform: translateY(0); }
.odometr-enter-to { transform: translateY(calc(-100% + 93px)); }

.animated {
  animation: bounce .2s 1;
}

@keyframes bounce {
  0% {
      transform: translateY(-7px);
  }
  20% {
      transform: translateY(-7px);
  }
  40% {
      transform: translateY(-6px);
  }
  60% {
      transform: translateY(-3px);
  }
  80% {
      transform: translateY(3px);
  }
  100% {
      transform: translateY(0);
  }
}

/* 414px  iPhone 6/7/8 */
@media (min-width: 414px) and (max-width: 812px) {
  .dice-honeycomb {
    height: 85px;
    margin-top: 13px;
  }

  .dice-honeycomb-digit {
    font-size: 53px;
    line-height: 80px;
  }

  .odometr-enter-to { transform: translateY(calc(-100% + 74px)); }

  @keyframes bounce {
    0% {
      transform: translateY(-6px);
    }
    20% {
      transform: translateY(-6px);
    }
    40% {
      transform: translateY(-5px);
    }
    60% {
      transform: translateY(-2px);
    }
    80% {
      transform: translateY(2px);
    }
    100% {
      transform: translateY(0);
    }
  }
}

/* 320px  iPhone 5/SE */
@media (min-width: 320px) and (max-width: 413px) {
  .dice-honeycomb {
    height: 65px;
    margin-top: 10px;
  }

  .dice-honeycomb-digit {
    font-size: 41px;
    line-height: 63px;
  }

  .odometr-enter-to { transform: translateY(calc(-100% + 58px)); }

  @keyframes bounce {
    0% {
      transform: translateY(-5px);
    }
    20% {
      transform: translateY(-5px);
    }
    40% {
      transform: translateY(-4px);
    }
    60% {
      transform: translateY(-2px);
    }
    80% {
      transform: translateY(2px);
    }
    100% {
      transform: translateY(0);
    }
  }
}
</style>
