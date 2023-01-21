<template>
  <div class="clock">
    <h1>{{ time }}</h1>
    <h2>Set alarm</h2>
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
    <button type="button" v-show="oscillator != null" @click="turnOffAlarm()">
      STOP ALARM
    </button>
  </div>
</template>
<script>
import { defineComponent } from 'vue'
import SelectComponent from './common/SelectComponent.vue'

export default defineComponent({
  name: 'AlarmClock',
  components: { SelectComponent },
  data () {
    return {
      selectedHour: NaN,
      selectedMinute: NaN,
      selectedSecond: NaN,
      hours: '00',
      minutes: '00',
      seconds: '00',
      time: '',
      oscillator: null,
      gainNode: null,
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
      }))
    }
  },
  created () {
    this.updateTime()
  },
  computed: {
    selectedTime () {
      return `${
        this.selectedHour <= 9 ? '0' + this.selectedHour : this.selectedHour
      }:${
        this.selectedMinute <= 9
          ? '0' + this.selectedMinute
          : this.selectedMinute
      }:${
        this.selectedSecond <= 9
          ? '0' + this.selectedSecond
          : this.selectedSecond
      }`
    }
  },
  watch: {
    time: {
      handler: function (time) {
        if (time === this.selectedTime) {
          this.turnOnAlarm()
        }
      },
      immediate: true
    }
  },
  methods: {
    updateTime () {
      const date = new Date()
      this.hours =
        date.getHours() <= 9 ? '0' + date.getHours() : date.getHours()
      this.minutes =
        date.getMinutes() <= 9 ? '0' + date.getMinutes() : date.getMinutes()
      this.seconds =
        date.getSeconds() <= 9 ? '0' + date.getSeconds() : date.getSeconds()
      this.time = `${this.hours}:${this.minutes}:${this.seconds}`
      requestAnimationFrame(this.updateTime)
    },
    getTime () {
      const date = new Date()

      this.hours =
        date.getHours() <= 9 ? '0' + date.getHours() : date.getHours()
      this.minutes =
        date.getMinutes() <= 9 ? '0' + date.getMinutes() : date.getMinutes()
      this.seconds =
        date.getSeconds() <= 9 ? '0' + date.getSeconds() : date.getSeconds()
      this.time = `${this.hours}:${this.minutes}:${this.seconds}`
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
    },
    turnOffAlarm () {
      this.oscillator.stop()
      this.oscillator.disconnect()
      this.gainNode.disconnect()
      this.oscillator = null
      this.gainNode = null
      this.selectedHour = NaN
      this.selectedMinute = NaN
      this.selectedSecond = NaN
    },
    selectedVal (selectedValue, label) {
      switch (label) {
        case 'hours':
          this.selectedHour = selectedValue
          break
        case 'minutes':
          this.selectedMinute = selectedValue
          break
        case 'seconds':
          this.selectedSecond = selectedValue
      }
    }
  }
})
</script>
<style scoped lang="scss">
h1 {
  font-size: 5.6rem;
}
h2 {
  font-size: 2.6rem;
}
h1,
h2,
h3,
p,
a {
  color: #fff;
}
.clock {
  display: grid;
  place-items: center;
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
button {
  background-color: #ad3434;
  padding: 15px 30px;
  margin-top: 50px;
  color: #fff;
  border-radius: 25px;
  border: 1px solid #fff;
  cursor: pointer;
  transition: all 0.5s;
  &:hover {
    background-color: #fff;
    color: #ad3434;
    border: 1px solid #ad3434;
  }
}
</style>
