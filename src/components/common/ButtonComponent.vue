<template>
  <button @click="emitEvent">{{ name }}</button>
</template>
<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'ButtonComponent',
  props: {
    name: String,
    active: Boolean
  },
  data () {
    return {
      scope: this.$props.name
    }
  },
  methods: {
    emitEvent () {
      this.$emit('toggle-active', this.scope)
    }
  }
})
</script>
