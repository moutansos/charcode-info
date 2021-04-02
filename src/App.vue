<template>
  <div class="app-container">
    <div class="app">
      <h1>charcode.info</h1>
      <BaseInput v-model:modelValue="inputText" />
      <TranslationTable :text="inputText" />
    </div>
  </div>
</template>

<script lang="ts">
import { computed, ComputedRef, defineComponent, Ref, ref } from "vue";
import BaseInput from "./components/BaseInput.vue";
import TranslationTable from "./components/TranslationTable.vue";

import _ from "lodash";

export default defineComponent({
  components: { BaseInput, TranslationTable },
  setup() {
    const inputText: Ref<string> = ref("");
    const decimalCodes: ComputedRef<number[]> = computed(() => {
      const length = inputText.value.length;
      return _.range(0, length).map((index) =>
        inputText.value.charCodeAt(index)
      );
    });
    const hexCodes = computed(() => {
      return decimalCodes.value.map((decimalValue) =>
        decimalValue.toString(16)
      );
    });
    return {
      inputText,
      decimalCodes,
      hexCodes,
    };
  },
});
</script>

<style>
.app-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 100vw;
  height: 100vh;
  color: white;
  background-image: linear-gradient(
    -45deg,
    rgb(0, 255, 255),
    rgb(51, 51, 250),
    rgb(158, 0, 158)
  );
  animation: gradient 60s ease infinite;
  background-size: 400% 400%;
}

@keyframes gradient {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

.app {
  min-width: 50vh;
}

h1 {
  font-family: Arial, Helvetica, sans-serif;
  text-transform: lowercase;
}
</style>
