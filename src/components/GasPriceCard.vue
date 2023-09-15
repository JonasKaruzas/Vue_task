<template>
  <div>Gas Price</div>
  <div>{{hello}}</div>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  setup() {
  
    const hello = 'hi'

    return {
      hello,
    }
  },
})
</script>
