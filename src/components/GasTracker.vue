<template>
  <div class="gas-tracker">
    <div v-if="gasPrices === undefined">No data</div>

    <div v-else class="gas-metrics">
      <div class="metric">
        <h2>Low Gas Price</h2>
        <p>{{ lowGasPrice }} Gwei</p>
      </div>
      <div class="metric">
        <h2>Average Gas Price</h2>
        <p>{{ averageGasPrice }} Gwei</p>
      </div>
      <div class="metric">
        <h2>High Gas Price</h2>
        <p>{{ highGasPrice }} Gwei</p>
      </div>
      <div class="metric">
        <h2>Base Price</h2>
        <p>{{ basePrice }} Gwei</p>
      </div>
      <div class="metric">
        <h2>Priority Price</h2>
        <p>{{ priorityPrice }} Gwei</p>
      </div>
      <div class="metric">
        <h2>Total Cost (USD)</h2>
        <p>{{ totalCostUSD }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, defineComponent, ref } from "vue";

export default defineComponent({
  name: "GasTracker",
  props: {
    gasPrices: Array,
  },
  setup(props) {
    const lowGasPrice = computed(() => props.gasPrices[0]?.low ?? 0);
    const basePrice = computed(() => props.gasPrices[0].mid ?? 0);
    const highGasPrice = computed(() => props.gasPrices[0].high ?? 0);
    const averageGasPrice = computed(() =>
      ((lowGasPrice.value + basePrice.value + highGasPrice.value) / 3).toFixed(
        5
      )
    );
    const priorityPrice = ref(35);
    const totalCostUSD = computed(() =>
      (averageGasPrice.value * 0.0338).toFixed(5)
    );

    return {
      lowGasPrice,
      averageGasPrice,
      highGasPrice,
      basePrice,
      priorityPrice,
      totalCostUSD,
    };
  },
});
</script>

<style scoped>
.gas-tracker {
  text-align: center;
  padding: 20px;
}

.gas-metrics {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.metric {
  margin: 20px;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 5px;
  min-width: 150px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.metric h2 {
  font-size: 18px;
  margin: 8px 16px;
}

.metric p {
  font-size: 24px;
  font-weight: bold;
  margin: 8px 16px;
}
</style>
