<template>
  <GasPriceCard />
</template>

<script>
import { defineComponent } from 'vue'
import GasPriceCard from './components/GasPriceCard.vue';

export default defineComponent({
  name: 'App',
  components: {
    GasPriceCard,
  },
  setup() {
  
  },
})
</script>
