<template>
  <div class="dice-score">
    <div class="left-corner">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="30" viewBox="0 0 24 30">
        <path class="svg-corner" :class="classObj" fill-rule="nonzero" d="M3 0A3 3 0 0 0 .504 4.664L7.394 15 .505 25.336A3 3 0 0 0 3 30h18a3 3 0 0 0 3-3v-1h-.999L23 27a2 2 0 0 1-1.85 1.995L21 29H3a2 2 0 0 1-1.664-3.11l6.89-10.335a1 1 0 0 0 0-1.11L1.337 4.11A2 2 0 0 1 3 1h8V0z" />
      </svg>
    </div>
    <!-- :style="borderStyle" -->
    <div
      class="dice-score-value"
      :class="classObj"
    >
      Выигрыш: <span class="value">{{ formatScore }} &#8381;</span>
    </div>
    <div class="right-corner">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="30" viewBox="0 0 24 30">
        <path class="svg-corner" :class="classObj" fill-rule="nonzero" d="M21 0a3 3 0 0 1 2.496 4.664L16.606 15l6.89 10.336A3 3 0 0 1 21 30H3a3 3 0 0 1-3-3v-1h.999L1 27a2 2 0 0 0 1.85 1.995L3 29h18a2 2 0 0 0 1.664-3.11l-6.89-10.335a1 1 0 0 1 0-1.11l6.89-10.336A2 2 0 0 0 21 1h-8V0z" />
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DiceScore',
  props: {
    score: {
      type: Number,
      required: true
    },
    resultType: {
      type: Number,
      required: true
    }
  },
  computed: {
    // Форматирует сумму
    formatScore () {
      return parseFloat(this.score).toFixed(2)
    },
    // Выставлеят класс для блока и корнеров в зависиомсти от типа результата
    classObj () {
      return {
        'default': this.resultType === 0,
        'win': this.resultType === 1,
        'lose': this.resultType === 2
      }
    }
  }
}
</script>

<style>
.dice-score {
  width: 237px;
  height: 30px;
  margin: 0 auto;
  position: relative;
}

.dice-score .dice-score-value {
  position: absolute;
  left: 13px;
  display: inline-block;
  width: 211px;
  height: 30px;
  line-height: 27px;
  border-radius: 5px;
  font-size: 12px;
  color: #a698af;
  text-align: center;
}

.dice-score .dice-score-value.default {
  transition: border-color .1s ease-out;
  border: solid 1px rgba(255, 255, 255, 0.1);
}

.dice-score .dice-score-value.win {
  transition: border-color .1s ease-out;
  border: solid 1px rgba(100, 255, 0, 0.3);
}

.dice-score .dice-score-value.lose {
  transition: border-color .1s ease-out;
  border: solid 1px rgba(255, 62, 62, 0.4);
}

.svg-corner.default {
  transition: all .1s ease-out;
  fill: #fff;
  fill-opacity: 0.1;
}

.svg-corner.win {
  transition: all .1s ease-out;
  fill: #64ff00;
  fill-opacity: 0.3;
}

.svg-corner.lose {
  transition: all .1s ease-out;
  fill: #ff3e3e;
  fill-opacity: 0.3;
}

.dice-score .dice-score-value .value {
  font-weight: bold;
  color: #fff;
}

.dice-score .left-corner,
.dice-score .right-corner {
  display: inline-block;
  width: 24px;
  height: 30px;
  position: absolute;
  top: 6px;
}

.dice-score .left-corner {
  left: 0;
}

.dice-score .right-corner {
  right: 0;
}

/* 414px  iPhone 6/7/8 */
@media (min-width: 414px) and (max-width: 812px) {
  .dice-score {
    width: 394px;
  }

  .dice-score .dice-score-value {
    width: 368px;
  }
}

/* 320px  iPhone 5/SE */
@media (min-width: 320px) and (max-width: 413px) {
  .dice-score {
    width: 299px;
  }

  .dice-score .dice-score-value {
    width: 273px;
  }
}
</style>
