<template>
  <div class="calculator">
    <div class="input-group">
      <label for="ip-input">IP адрес:</label>
      <input
        id="ip-input"
        v-model="ipAddress"
        type="text"
        placeholder="192.168.1.150"
        class="ip-input"
        @keyup.enter="calculate"
      >
    </div>

    <div class="input-group">
      <label for="mask-select">Маска сети:</label>
      <select
        id="mask-select"
        v-model="selectedMask"
        class="mask-select"
      >
        <option
          v-for="mask in NETWORK_MASKS"
          :key="mask.value"
          :value="mask.value"
        >
          {{ mask.label }}
        </option>
      </select>
    </div>

    <button
      :disabled="!isValid"
      class="calculate-btn"
      @click="calculate"
    >
      Рассчитать
    </button>

    <div
      v-if="showResults"
      class="results"
    >
      <h3>Результаты:</h3>
      <div class="result-item">
        <strong>IP адрес:</strong> {{ ipAddress }}
      </div>
      <div class="result-item">
        <strong>Маска сети:</strong> {{ selectedMaskLabel }}
      </div>
      <div class="result-item">
        <strong>Адрес сети:</strong> {{ networkAddress }}
      </div>
      <div class="result-item">
        <strong>Количество адресов:</strong> {{ addressesCount }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { NETWORK_MASKS } from '../../constants/networkMasks';
import { isIpValid } from '../../utils/ipValidation';
import { getNetworkAddress, getAddressesCount } from '../../utils/networkCalculations';

const ipAddress = ref('');
const selectedMask = ref('255.255.255.0');
const networkAddress = ref('');
const addressesCount = ref(0);
const showResults = ref(false);

const isValid = computed(() => isIpValid(ipAddress.value));

const selectedMaskLabel = computed(() => {
  const mask = NETWORK_MASKS.find(m => m.value === selectedMask.value);
  return mask ? mask.label : '';
});

function calculate() {
  if (!isValid.value) return;

  networkAddress.value = getNetworkAddress(ipAddress.value, selectedMask.value);
  addressesCount.value = getAddressesCount(selectedMask.value);
  showResults.value = true;
}
</script>

<style scoped>
.calculator {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.input-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 16px;
}

label {
  margin-bottom: 8px;
  font-weight: bold;
  color: #333;
}

.ip-input,
.mask-select {
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.calculate-btn {
  width: 100%;
  padding: 12px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.calculate-btn:disabled {
  background-color: #6c757d;
  cursor: not-allowed;
}

.calculate-btn:not(:disabled):hover {
  background-color: #0056b3;
}

.results {
  margin-top: 24px;
  padding: 16px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: #f8f9fa;
}

.result-item {
  margin-bottom: 8px;
  padding: 4px 0;
}

h3 {
  margin-top: 0;
  margin-bottom: 16px;
  color: #333;
}
</style>