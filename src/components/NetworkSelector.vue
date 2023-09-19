<template>
  <div class="network-selector">
    <h2>Select Network</h2>
    <div class="network-buttons">
      <button
        v-for="network in networks"
        :key="network.id"
        @click="onSelectNetwork(network.id)"
        :class="{ active: selectedNetwork === network.id }"
      >
        {{ network.name }}
      </button>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "NetworkSelector",
  props: {
    selectedNetwork: String,
    onSelectNetwork: Function,
    networks: Array,
  },
  setup() {
    return {};
  },
});
</script>

<style scoped>
.network-selector {
  text-align: center;
  padding: 20px;
}

.network-buttons {
  margin-top: 20px;
}

.network-buttons button {
  margin: 0 10px;
  padding: 10px 20px;
  border: none;
  background-color: #007bff;
  color: #fff;
  border-radius: 5px;
  cursor: pointer;
}

.network-buttons button.active {
  background-color: #0056b3;
}
</style>
