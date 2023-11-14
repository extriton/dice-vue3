<template>
  <div class="dice-game">
    <!-- Score -->
    <div class="dice-score-wrapper">
      <dice-score
        :score="score"
        :result-type="resultType"
      />
    </div>
    <!-- Result block -->
    <div class="dice-result-block">
      <!-- User Value -->
      <div class="dice-user-value-wrapper">
        <dice-user-value
          :user-value="userValue"
          :reversed="reversed"
        />
      </div>
      <!-- Game result -->
      <div class="dice-result-wrapper">
        <dice-result
          :game-value="gameValue"
          :game-counter="gameCounter"
          :result-type="resultType"
        />
      </div>
      <!-- History -->
      <div class="dice-history-wrapper">
        <dice-history
          :history="history"
          :game-counter="gameCounter"
        />
        <div class="dice-reverse-wrapper">
          <dice-reverse
            @reverse="reverse"
          />
        </div>
      </div>
      <div class="clearfix" />
    </div>
    <!-- Slider -->
    <div class="dice-slider-wrapper">
      <dice-slider
        :user-value="userValue"
        :game-value="gameValue"
        :reversed="reversed"
        :game-counter="gameCounter"
        @changeUserValue="changeUserValue"
      />
      <div class="dice-reverse-wrapper">
        <dice-reverse
          @reverse="reverse"
        />
      </div>
    </div>
    <!-- Button -->
    <div class="dice-play-button-wrapper">
      <dice-play-button
        @dicePlay="dicePlay"
      />
    </div>
  </div>
</template>

<script>
import DiceScore from '@/components/DiceScore.vue'
import DiceUserValue from '@/components/DiceUserValue.vue'
import DiceResult from '@/components/DiceResult.vue'
import DiceHistory from '@/components/DiceHistory.vue'
import DiceSlider from '@/components/DiceSlider.vue'
import DiceReverse from '@/components/DiceReverse.vue'
import DicePlayButton from '@/components/DicePlayButton.vue'

import { generateRandom } from '@/helpers/generate-random'

export default {
  name: 'DiceGame',
  components: {
    DiceScore,
    DiceUserValue,
    DiceResult,
    DiceHistory,
    DiceSlider,
    DiceReverse,
    DicePlayButton
  },
  data () {
    return {
      userValue: 50,
      gameValue: 0,
      reversed: false,
      score: 350.2,
      resultType: 0, // resultType: 0 - default, 1 - win, 2 - lose
      history: [],
      gameCounter: 0
    }
  },
  methods: {
    // Реверс: больше - меньше
    reverse () {
      this.reversed = !this.reversed
    },
    // Устанавливает новое занчение userValue по событию из слайдера
    changeUserValue (value) {
      this.userValue = value
    },
    // Начинает розыгрыш
    dicePlay () {
      // Сбрасываем в дефолт стили и классы элементов
      this.resultType = 0
      // Эмуляция получения ответа с сервера
      this.onServerResponse()
    },
    // Эмуляция получения ответа с сервера
    onServerResponse () {
      this.gameValue = generateRandom()
      this.gameCounter++

      let tmp
      tmp = this.gameValue / 100
      if ((tmp <= this.userValue && this.reversed) || (tmp >= this.userValue && !this.reversed)) {
        tmp *= -1
      }
      this.history.push(tmp)

      setTimeout(() => {
        const snd = new Audio()
        if (tmp > 0) {
          this.resultType = 1
          snd.src = '/snd/Win.mp3'
        } else {
          this.resultType = 2
          snd.src = '/snd/Lose.mp3'
        }
        snd.volume = 0.4
        snd.autoplay = true
      }, 1200)
    }
  }
}
</script>

<style>
@import url('@/assets/fonts/source_sans_pro.css');

.dice-game {
  width: 813px;
  padding: 18px 35px 35px 35px;
  background-color: #291a43;
  border: 1px solid #000000;
  border-radius: 7px;
  font-family: "Source Sans Pro", sans-serif;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  letter-spacing: normal;
}

.dice-score-wrapper {
  margin-bottom: 29px;
}

.dice-result-block {
  margin-bottom: 45px;
}

.dice-user-value-wrapper {
  width: 121px;
  float: left;
}

.dice-result-wrapper {
  width: 504px;
  float: left;
}

.dice-history-wrapper {
  width: 53px;
  float: right;
}

.dice-history-wrapper .dice-reverse-wrapper {
  display: none;
}

.dice-slider-wrapper {
  position: relative;
  margin-bottom: 44px;
}

.dice-slider-wrapper .dice-reverse-wrapper {
  width: 34px;
  height: 34px;
  position: absolute;
  left: 50%;
  bottom: 0;
  transform: translate(-50%, 50%);
}

.clearfix {
  clear: both;
}

/* 414px  iPhone 6/7/8 */
@media (min-width: 414px) and (max-width: 812px) {
  .dice-game {
    width: 414px;
    border: 0;
    border-radius: 0;
    padding: 15px 10px 15px 9px;
  }

  .dice-score-wrapper {
    margin-bottom: 15px;
  }

  .dice-result-block {
    margin-bottom: 15px;
  }

  .dice-user-value-wrapper {
    width: 100%;
    float: none;
    margin-bottom: 5px;
  }

  .dice-result-wrapper {
    width: 100%;
    float: none;
    margin-bottom: 40px;
  }

  .dice-history-wrapper {
    width: 100%;
    float: none;
    position: relative;
  }

  .dice-history-wrapper .dice-reverse-wrapper {
    display: block;
    width: 34px;
    height: 34px;
    position: absolute;
    right: -5px;
    top: -5px;
  }

  .dice-slider-wrapper {
    margin-bottom: 14px;
  }

  .dice-slider-wrapper .dice-reverse-wrapper {
    display: none;
  }
}

/* 320px  iPhone 5/SE */
@media (min-width: 320px) and (max-width: 413px) {
  .dice-game {
    width: 320px;
    border: 0;
    border-radius: 0;
    padding: 15px 0 20px 0;
  }

  .dice-score-wrapper {
    margin-bottom: 16px;
  }

  .dice-result-block {
    margin-bottom: 15px;
  }

  .dice-user-value-wrapper {
    width: 100%;
    float: none;
    margin-bottom: 8px;
  }

  .dice-result-wrapper {
    width: 100%;
    float: none;
    margin-bottom: 55px;
    padding: 0 9px 0 10px;
  }

  .dice-history-wrapper {
    width: 100%;
    float: none;
    position: relative;
    padding: 0 9px 0 10px;
  }

  .dice-history-wrapper .dice-reverse-wrapper {
    display: block;
    width: 34px;
    height: 34px;
    position: absolute;
    right: 5px;
    top: -5px;
  }

  .dice-slider-wrapper {
    margin-bottom: 15px;
  }

  .dice-slider-wrapper .dice-reverse-wrapper {
    display: none;
  }

  .dice-play-button-wrapper {
    padding: 0 9px;
  }
}
</style>
