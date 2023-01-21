<template>
  <HeaderWrapper @toggle-active="toggleActive" />
  <TransitionGroup>
    <AlarmClock v-if="scope === 'AlarmClock'" />
    <StopWatch v-if="scope === 'Stopwatch'"/>
    <TimerComponent v-if="scope === 'Timer'" />
  </TransitionGroup>
</template>

<script>
import TimerComponent from './components/TimerComponent.vue'
import HeaderWrapper from '@/components/HeaderWrapper.vue'
import AlarmClock from './components/AlarmClock.vue'
import StopWatch from './components/StopWatch.vue'

export default {
  name: 'App',
  components: {
    TimerComponent,
    HeaderWrapper,
    AlarmClock,
    StopWatch
  },
  data () {
    return {
      scope: String
    }
  },
  methods: {
    toggleActive (value) {
      this.scope = value
    }
  }
}
</script>

<style lang="scss">
.v-enter-active {
  transition: opacity 1s ease;
}
.v-enter-from {
  opacity: 0;
}
html {
  font-size: 10px;
}
body {
  margin: 0;
  padding: 0;
  background-color: #0e0d0d;
}
ul, ol, li {
  list-style: none;
  padding: 0;
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
