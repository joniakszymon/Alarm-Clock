<template>
    <select @change="this.$emit('selectedVal', { selectedValue, label })" v-model="selectedValue">
      <option v-for="option in options" :key="option.value" :value="option.value">
        {{ option.label }}
      </option>
    </select>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'SelectComponent',
  props: {
    options: {
      type: Array,
      required: true
    },
    label: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      selectedValue: NaN
    }
  }
})
</script>
