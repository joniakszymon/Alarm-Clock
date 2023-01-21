<template>
  <div class="stopwatch">
    <h1>
      {{
        `${minutes <= 9 ? '0' + minutes : minutes}:${
          seconds <= 9 ? '0' + seconds : seconds
        },${miliseconds <= 9 ? '0' + miliseconds : miliseconds}`
      }}
    </h1>
    <div class="control-box">
      <button v-if="!isStarted" @click="startTimer" id="start">Start</button>
      <button v-if="isStarted" @click="stopTimer" id="stop">Stop</button>
      <button v-if="isStarted" @click="saveTime" id="round">Runda</button>
      <button v-if="!isStarted" @click="resetTimer" id="reset">Wyzeruj</button>
    </div>
    <div class="times-list" v-show="timesList.length !== 0">
      <h2>Lista czasów:</h2>
      <ul>
        <li
          v-for="time in timesList"
          :key="timesList.indexOf(time)"
        >
         {{ `${timesList.indexOf(time) + 1}. ${time}` }}
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'StopWatch',
  data () {
    return {
      seconds: 0,
      minutes: 0,
      miliseconds: 0,
      intervalId: null,
      isStarted: false,
      timesList: []
    }
  },
  methods: {
    startTimer () {
      this.isStarted = true
      this.intervalId = setInterval(() => {
        this.miliseconds++
        if (this.miliseconds === 100) {
          this.miliseconds = 0
          this.seconds++
        }
        if (this.seconds === 60) {
          this.seconds = 0
          this.minutes++
        }
      }, 10)
    },
    stopTimer () {
      this.isStarted = false
      clearInterval(this.intervalId)
    },
    resetTimer () {
      this.seconds = this.minutes = this.miliseconds = '0'
      this.timesList = []
    },
    saveTime () {
      const currentTime = `${this.minutes <= 9 ? '0' + this.minutes : this.minutes}:${
        this.seconds <= 9 ? '0' + this.seconds : this.seconds
      },${this.miliseconds <= 9 ? '0' + this.miliseconds : this.miliseconds}`
      this.timesList.push(currentTime)
    }
  }
})
</script>
<style scoped lang="scss">
$button-width: 120px;
h1 {
  font-size: 5.6rem;
  color: #fff;
}
.stopwatch {
  display: grid;
  place-items: center;
  max-width: 600px;
  margin: auto;
  .control-box {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: calc(2 * $button-width + 20px);
    button {
      width: $button-width;
      padding: 10px 0;
      border: 1px solid #fff;
      color: #fff;
      border-radius: 25px;
      &#start {
        background-color: #2b7219;
      }
      &#stop {
        background-color: #a93939;
      }
      &#reset,
      &#round {
        background-color: #4b4b4b;
      }
      &:hover {
        cursor: pointer;
      }
    }
  }
  .times-list {
      margin-top: 60px;
      padding: 0 30px 30px;
      border: 1px double #fff;
      width: 100%;
    h2 {
      color: #fff;
      font-size: 2.4rem;
    }
    ul > li {
      color: #fff;
      font-size: 20px;
      padding: 10px 0;
      &:not(:last-of-type) {
        border-bottom: 1px solid #fff;
      }
    }
  }
}
@media screen and (max-width: 800px) {
  .stopwatch {
    .times-list {
        width: auto;
    }
  }
}
</style>
