<template>
  <div
    class="dice-slider"
  >
    <ul class="caption">
      <li
        v-for="item in captionItems"
        :key="'c' + item"
        class="caption-item"
        :style="{ left: item + '%' }"
      >
        {{ item }}
      </li>
    </ul>
    <transition name="line-fade">
      <div
        v-if="gameCounter > 0"
        class="rule-line"
        :style="setLineStyle()"
      ></div>
    </transition>
    <div class="rule-wrapper">
      <div
        v-show="drag"
        class="runner-pad"
        @mousemove="doDrag"
        @mouseup="stopDrag"
        @mouseleave="stopDrag"
      ></div>
      <div
        class="runner-touch-wrap"
        :style="{ left: userValue + '%' }"
        @touchstart="startDrag"
        @touchmove="doDrag"
        @touchend="stopDrag"
      >
        <div
          class="runner"
          :class="{ dragged: drag }"
          @mousedown="startDrag"
        >
          {{ runnerValue }}
        </div>
      </div>
      <ul
        ref="rule"
        class="rule"
        @click.capture="shiftRunner"
      >
        <li
          v-for="item in (ruleStyles.ruleLineCount + 1)"
          :key="'n' + item"
          class="rule-item"
          :style="setRuleItemStyle(item - 1)"
        ></li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DiceSlider',
  props: {
    userValue: {
      type: Number,
      required: true
    },
    gameValue: {
      type: Number,
      required: true
    },
    reversed: {
      type: Boolean,
      required: true
    },
    gameCounter: {
      type: Number,
      required: true
    }
  },
  data () {
    return {
      captionItems: [0, 25, 50, 75, 100],
      drag: false,
      ruleWidth: 0,
      startDragX: 0,
      startDragValue: 0,
      ruleStyles: {
        ruleOffset: 0,
        ruleLineCount: 0,
        pixelsByRuleLine: 0
      }
    }
  },
  computed: {
    // Возвращает значение отображаемое внутри триггера если он перетягивается мышкой
    runnerValue () {
      if (this.drag) {
        return parseInt(this.userValue)
      } else {
        return ''
      }
    }
  },
  mounted () {
    this.calcRuleParams()
    window.addEventListener('resize', this.calcRuleParams)
  },
  methods: {
    calcRuleParams () {
      this.ruleWidth = this.$refs.rule.getBoundingClientRect().width

      // PC mode
      if (this.ruleWidth === 702) {
        this.ruleStyles.ruleOffset = 21
        this.ruleStyles.ruleLineCount = 100
        this.ruleStyles.pixelsByRuleLine = 7
      }

      // Mobile mode iPhone 6/7/8 (414px)
      if (this.ruleWidth === 362) {
        this.ruleStyles.ruleOffset = 17
        this.ruleStyles.ruleLineCount = 60
        this.ruleStyles.pixelsByRuleLine = 6
      }

      // Mobile mode iPhone 5/SE (320px)
      if (this.ruleWidth === 302) {
        this.ruleStyles.ruleOffset = 9
        this.ruleStyles.ruleLineCount = 50
        this.ruleStyles.pixelsByRuleLine = 6
      }
    },
    // Позиционирует линию
    setLineStyle () {
      const style = {}
      const calcGameValue = this.gameValue / 100 * (this.ruleStyles.ruleLineCount / 100 * this.ruleStyles.pixelsByRuleLine)

      if (this.gameValue >= 0) {
        style.left = this.ruleStyles.ruleOffset + calcGameValue + 'px'
      } else {
        style.opacity = 1
      }

      if ((this.gameValue / 100 <= this.userValue && this.reversed) || (this.gameValue / 100 >= this.userValue && !this.reversed)) {
        style.backgroundColor = 'red'
      } else {
        style.backgroundColor = '#50CC00'
      }

      return style
    },
    // Позиционирует деления линейки
    setRuleItemStyle (value) {
      const style = {}
      const calcUserValue = this.userValue / 100 * this.ruleStyles.ruleLineCount

      if ((value <= calcUserValue && this.reversed) || (value >= calcUserValue && !this.reversed)) {
        style.backgroundColor = 'red'
      }

      let dx = 0
      dx = Math.abs(value - calcUserValue)
      if (dx <= 4) {
        const y = Math.sqrt(16 - dx * dx)
        let tmp = 0
        tmp = -7 + y * 4
        if (dx > 3) {
          tmp = tmp * 1 - (3 - dx) * 1.5
        } else {
          tmp = tmp * 1 + (3 - dx) * 1.5
        }
        if (tmp < 0) { tmp = 0 }
        style.top = tmp + 'px'
      }

      return style
    },
    // Начало перетягивания триггера мышкой
    startDrag (e) {
      this.drag = true
      e.preventDefault()
      e = e.changedTouches ? e.changedTouches[0] : e
      this.startDragX = e.clientX
      this.startDragValue = this.userValue
      this.ruleWidth = this.$refs.rule.getBoundingClientRect().width
    },
    // Завршение перетягивания триггера мышкой
    stopDrag (e) {
      this.drag = false
    },
    // Перетягивание триггера мышкой
    doDrag (e) {
      if (this.drag) {
        e = e.changedTouches ? e.changedTouches[0] : e
        let newValue = this.startDragValue + (e.clientX - this.startDragX) / this.ruleWidth * 100
        newValue = parseInt(newValue * 100) / 100
        if (newValue < 2) { newValue = 2 }
        if (newValue > 98) { newValue = 98 }
        this.changeUserValue(newValue)
      }
    },
    // Перемещение триггера при клике по линейке
    shiftRunner (e) {
      let newValue
      e = e.changedTouches ? e.changedTouches[0] : e

      if (e.target._prevClass === 'rule-item') {
        newValue = parseInt((e.offsetX + e.target.offsetLeft) / e.target.parentNode.offsetWidth * 10000) / 100
      } else {
        newValue = parseInt(e.offsetX / e.target.offsetWidth * 10000) / 100
      }

      if (newValue < 2) { newValue = 2 }
      if (newValue > 98) { newValue = 98 }

      const deltaValue = (newValue - this.userValue) / 20
      const intervalId = setInterval(() => {
        if (this.userValue === newValue) {
          clearInterval(intervalId)
          return
        }
        if ((deltaValue > 0 && this.userValue + deltaValue > newValue) ||
          (deltaValue < 0 && this.userValue + deltaValue < newValue)) {
          this.changeUserValue(newValue)
        } else {
          this.changeUserValue(this.userValue + deltaValue)
        }
      }, 10)
    },
    // Эмит события родителю на изменение userValue
    changeUserValue (value) {
      this.$emit('changeUserValue', value)
    }
  }
}
</script>

<style>
.dice-slider {
  height: 80px;
  border-radius: 5px;
  background-color: rgba(0, 0, 0, 0.25);
  padding: 12px 19px 0 20px;
  position: relative;
}

.dice-slider .caption {
  position: relative;
  height: 16px;
}

.dice-slider .caption .caption-item {
  position: absolute;
  font-size: 12px;
  line-height: 16px;
  color: #a698af;
  opacity: 0.5;
  transform: translateX(-50%);
  list-style-type: none;
}

.dice-slider .caption .caption-item:first-child {
  transform: translateX(0);
}

.dice-slider .caption .caption-item:last-child {
  transform: translateX(-100%);
}

.dice-slider .rule-line {
  position: absolute;
  top: 0;
  width: 2px;
  height: 80px;
  background-color: #50CC00;
  transition: all .2s ease-out 1.3s;
}

.dice-slider .rule-wrapper {
  position: relative;
  margin-top: 12px;
  z-index: 5;
}

.dice-slider .rule-wrapper .rule {
  position: relative;
  width: 100%;
  height: 23px;
}

.dice-slider .rule-wrapper .rule:hover {
  cursor: pointer;
}

.dice-slider .rule-wrapper .rule .rule-item {
  display: inline-block;
  position: relative;
  margin-right: 5px;
  top: 0;
  width: 2px;
  height: 8px;
  background-color: #50CC00;
}

.dice-slider .rule-wrapper .rule .rule-item:last-child {
  margin-right: 0;
}

.runner-touch-wrap {
  width: 28px;
  height: 48px;
  position: absolute;
  top: -40px;
  transform: translateX(-50%);
}

.dice-slider .rule-wrapper .runner {
  position: absolute;
  bottom: 0;
  width: 28px;
  height: 28px;
  line-height: 28px;
  font-size: 12px;
  border-radius: 100%;
  color: #291944;
  text-align: center;
  background: url('@/assets/arrows.svg') no-repeat #FFF;
  background-position: 4px 10px;
  z-index: 10;
}

.dice-slider .rule-wrapper .runner:hover {
  background-color: #FFEF00;
  cursor: pointer;
}

.dice-slider .rule-wrapper .runner.dragged {
  background-color: #FFEF00;
  cursor: pointer;
  background-image: none;
}

.dice-slider .rule-wrapper .runner-pad {
  position: absolute;
  top: -120px;
  bottom: -100px;
  left: -56px;
  right: -55px;
  z-index: 15;
}

/* Transition styles */
.line-fade-enter-active, .fade-leave-active {
  transition: opacity .2s ease-out 1.3s;
}
.line-fade-enter { opacity: 0; }
.line-fade-enter-to { opacity: 1; }

/* 414px  iPhone 6/7/8 */
@media (min-width: 414px) and (max-width: 812px) {
  .dice-slider {
    height: 75px;
    padding: 12px 16px 0 17px;
  }

  .dice-slider .rule-wrapper .rule .rule-item {
    margin-right: 4px;
  }

  .dice-slider .rule-line {
    height: 75px;
  }

  .dice-slider .rule-wrapper .runner-pad {
    top: -120px;
    bottom: -90px;
    left: -26px;
    right: -26px;
  }
}

/* 320px  iPhone 5/SE */
@media (min-width: 320px) and (max-width: 413px) {
  .dice-slider {
    height: 75px;
    padding: 12px 9px 0 9px;
  }

  .dice-slider .rule-wrapper .rule .rule-item {
    margin-right: 4px;
  }

  .dice-slider .rule-line {
    height: 75px;
  }

  .dice-slider .rule-wrapper .runner-pad {
    top: -120px;
    bottom: -90px;
    left: -9px;
    right: -9px;
  }
}
</style>
