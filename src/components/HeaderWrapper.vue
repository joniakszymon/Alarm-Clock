<template>
  <header>
    <ul>
      <li>
        <ButtonComponent
          @toggle-active="toggleActive"
          :name="'AlarmClock'" />
      </li>
      <li>
        <ButtonComponent
          @toggle-active="toggleActive"
          :name="'Stopwatch'"
        />
      </li>
      <li>
        <ButtonComponent
          @toggle-active="toggleActive"
          :name="'Timer'"
        />
      </li>
    </ul>
  </header>
</template>
<script>
import { defineComponent } from 'vue'
import ButtonComponent from './common/ButtonComponent.vue'

export default defineComponent({
  name: 'HeaderWrapper',
  components: { ButtonComponent },
  methods: {
    toggleActive (value) {
      this.$emit('toggle-active', value)
      this.active = true
    }
  }
})
</script>
<style scoped lang="scss">
header {
  display: grid;
  place-items: center;
  margin: auto;
  height: 120px;
  ul {
    display: grid;
    place-items: center;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: 1;
    column-gap: 40px;
    li > button {
      background-color: transparent;
      color: #fff;
      border: 1px solid #fff;
      border-radius: 25px;
      padding: 10px 15px;
      width: 220px;
      transition: all 0.5s;
      font-size: 1.6rem;
      &:hover {
        cursor: pointer;
        background-color: #ad3434;
      }
    }
  }
}
@media screen and (max-width: 800px) {
  header {
    height: 100%;
    margin: 10px 0;
    ul {
      grid-template-columns: 1fr;
      column-gap: 0;
      row-gap: 15px;
    }
  }
}
</style>
