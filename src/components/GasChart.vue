<template>
  <div class="gas-chart">
    <h2>Gas Price History</h2>

    <div v-if="!chartSeries">No data</div>

    <div v-else class="chart">
      <apexchart
        type="line"
        :options="chartOptions"
        :series="chartSeries"
        width="100%"
        height="400"
      />
    </div>
    <div class="timeframe-switcher">
      <button
        v-for="timeFrame in timeFrames"
        :key="timeFrame"
        @click="switchTimeframe(timeFrame)"
        :class="{ active: selectedTimeframe === timeFrame }"
      >
        {{ timeFrame }} Days
      </button>
    </div>
  </div>
</template>

<script>
import VueApexCharts from "vue3-apexcharts";
import { defineComponent, ref, computed } from "vue";

export default defineComponent({
  name: "GasChart",
  components: {
    apexchart: VueApexCharts,
  },
  props: {
    gasPrices: {
      type: Array,
      default: () => [{}],
    },
  },
  setup(props) {
    const timeFrames = [7, 30, 90];

    const chartOptions = {
      chart: {
        zoom: {
          enabled: false,
        },
      },
      xaxis: {
        type: "datetime",
      },
    };

    const selectedTimeframe = ref(90);

    const chartDataLength = computed(() =>
      props.gasPrices.slice(0, selectedTimeframe.value)
    );

    const chartSeries = computed(() =>
      Object.keys(chartDataLength.value[0])
        .filter((key) => key !== "timestamp")
        .map((priceType) => {
          return {
            name: `${
              priceType.charAt(0).toUpperCase() + priceType.slice(1)
            } Price`,
            data: chartDataLength.value.map((item) => [
              new Date(item.timestamp).getTime(),
              item[priceType],
            ]),
          };
        })
    );

    const switchTimeframe = (days) => {
      selectedTimeframe.value = days;
    };

    return {
      chartOptions,
      chartSeries,
      switchTimeframe,
      timeFrames,
      selectedTimeframe,
    };
  },
});
</script>

<style scoped>
.gas-chart {
  text-align: center;
  padding: 20px;
}

.chart {
  margin: 20px;
}

.timeframe-switcher {
  margin-top: 20px;
}

.timeframe-switcher button {
  margin: 0 10px;
  padding: 10px 20px;
  border: none;
  background-color: #007bff;
  color: #fff;
  border-radius: 5px;
  cursor: pointer;
}

.timeframe-switcher button:hover {
  background-color: #0056b3;
}
.timeframe-switcher button.active {
  background-color: #0056b3;
}
</style>
