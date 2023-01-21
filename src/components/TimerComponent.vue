<template>
  <div class="timer">
    <h1>
      {{ selectedHour <= 9 ? `0${selectedHour}` : selectedHour }}:{{
        selectedMinute <= 9 ? `0${selectedMinute}` : selectedMinute
      }}:{{ selectedSecond <= 9 ? `0${selectedSecond}` : selectedSecond }}
    </h1>
    <div class="inputs">
      <SelectComponent
        @selectedVal="selectedVal"
        v-model="selectedHour"
        :options="hoursOptions"
        label="hours"
      />
      <SelectComponent
        @selectedVal="selectedVal"
        v-model="selectedMinute"
        :options="minutesOptions"
        label="minutes"
      />
      <SelectComponent
        @selectedVal="selectedVal"
        v-model="selectedSecond"
        :options="secondsOptions"
        label="seconds"
      />
    </div>
    <div class="control-box">
      <button @click="startTimer">Start</button>
      <button @click="stopTimer">Stop</button>
    </div>
  </div>
</template>

<script>
import SelectComponent from './common/SelectComponent.vue'

export default {
  name: 'TimerComponent',
  components: { SelectComponent },
  data () {
    return {
      selectedHour: '0',
      selectedMinute: '0',
      selectedSecond: '0',
      secondsOptions: Array.from({ length: 60 }, (_, i) => ({
        label: i < 10 ? `0${i}` : i,
        value: i
      })),
      minutesOptions: Array.from({ length: 60 }, (_, i) => ({
        label: i < 10 ? `0${i}` : i,
        value: i
      })),
      hoursOptions: Array.from({ length: 24 }, (_, i) => ({
        label: i < 10 ? `0${i}` : i,
        value: i
      })),
      intervalId: null,
      isStarted: false
    }
  },
  methods: {
    startTimer () {
      this.isStarted = true
      this.intervalId = setInterval(() => {
        this.selectedSecond--
        if (this.selectedSecond < 0) {
          this.selectedSecond = 59
          this.selectedMinute--
        }
        if (this.selectedMinute < 0) {
          this.selectedMinute = 59
          this.selectedHour--
        }
        if (this.selectedHour < 0) {
          this.stopTimer()
          this.turnOnAlarm()
          this.selectedHour = this.selectedMinute = this.selectedSecond = 0
        }
      }, 1000)
    },
    stopTimer () {
      this.isStarted = false
      clearInterval(this.intervalId)
    },
    resetTimer () {
      this.seconds = this.minutes = this.miliseconds = '0'
      this.timesList = []
    },
    turnOnAlarm () {
      const audioContext = new (window.AudioContext ||
        window.webkitAudioContext)()
      this.oscillator = audioContext.createOscillator()
      this.gainNode = audioContext.createGain()
      this.oscillator.connect(this.gainNode)
      this.gainNode.connect(audioContext.destination)

      this.oscillator.type = 'sine'
      this.oscillator.frequency.value = 440

      this.oscillator.start()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.timer {
  display: grid;
  place-items: center;
  h1 {
    font-size: 5.6rem;
    color: #fff;
  }
  .inputs {
    display: flex;
    select {
    padding: 15px 30px;
    border: 2px solid #ad3434;
    &:nth-of-type(1) {
      border-right: none;
    }
    &:nth-of-type(2) {
      border-right: none;
      border-left: none;
    }
    &:nth-of-type(3) {
      border-left: none;
    }
  }
  }
}
</style>
