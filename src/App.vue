<template>
  <NetworkSelector :selectedNetwork="selectedNetwork" :onSelectNetwork="onSelectNetwork" :networks="networks" />
  <GasTracker :gasPrices="gasPrices" />
  <GasChart :gasPrices="gasPrices" />
</template>

<script>
import { computed, defineComponent, onBeforeMount, ref } from "vue";
import GasChart from "./components/GasChart.vue";
import GasTracker from "./components/GasTracker.vue";
import NetworkSelector from "./components/NetworkSelector.vue";
import axios from "axios";

export default defineComponent({
  name: "App",
  components: {
    GasTracker,
    NetworkSelector,
    GasChart,
  },
  setup() {
    const selectedNetwork = ref("ethereum");

    const onSelectNetwork = (networkId) => {
      selectedNetwork.value = networkId;
    };

    const networks = [
      { id: "ethereum", name: "Ethereum" },
      { id: "bsc", name: "Binance Smart Chain" },
      { id: "polygon", name: "Polygon" },
    ];

    const allGasPrices = ref([]);

    const fetchGasPrices = async (network) => {
      try {
        const res = await axios.get(`/gasPriceData_${network}.json`);
        return res.data.gasPrices;
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    };

    const gasPrices = computed(() => allGasPrices.value.find((item) => item.id === selectedNetwork.value)?.data);

    onBeforeMount(async () => {
      const promises = networks.map(async (network) => {
        const gasPrices = await fetchGasPrices(network.id);
        return { id: network.id, data: gasPrices };
      });

      const results = await Promise.all(promises);

      allGasPrices.value = results;
    });

    return {
      selectedNetwork,
      onSelectNetwork,
      networks,
      gasPrices,
      allGasPrices,
    };
  },
});
</script>
