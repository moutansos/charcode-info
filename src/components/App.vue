<template>
  <div class="app-container">
    <div class="app">
      <h1>charcode.info</h1>
      <BaseInput v-model:modelValue="inputText" />
      <TranslationTable
        v-for="(row, i) in inputTextChunks"
        :key="i"
        :text="row"
      />
    </div>
    <footer>
      Check out my blog:
      <a class="link" href="https://benbrougher.tech">benbrougher.tech</a>
    </footer>
  </div>
</template>

<script setup lang="ts">
import {
  computed,
  ComputedRef,
  defineComponent,
  Ref,
  ref,
  onMounted,
} from "vue";
import BaseInput from "./BaseInput.vue";
import TranslationTable from "./TranslationTable.vue";

import _ from "lodash";

function chunkString(str, length) {
  if (!str) return [];
  return str.match(new RegExp(".{1," + length + "}", "g"));
}
const windowWidth: Ref<number> = ref(0);

onMounted(() => {
  windowWidth.value = window.innerWidth;
  window.onresize = () => {
    console.log(`Window resizing: ${window.innerWidth}`);
    windowWidth.value = window.innerWidth;
  };

  setInterval(() => {
    windowWidth.value = window.innerWidth;
  }, 100);
});

const inputText: Ref<string> = ref("");
const chunkSize: ComputedRef<number> = computed(() => {
  const width = windowWidth.value - 75;
  if (width <= 0) return 3;
  const result = Math.floor(width / 40);
  return result > 50 ? 50 : result;
});
const inputTextChunks: ComputedRef<string[]> = computed(() =>
  chunkString(inputText.value, chunkSize.value)
);
const decimalCodes: ComputedRef<number[]> = computed(() => {
  const length = inputText.value.length;
  return _.range(0, length).map((index) => inputText.value.charCodeAt(index));
});
const hexCodes = computed(() => {
  return decimalCodes.value.map((decimalValue) => decimalValue.toString(16));
});
</script>

<style>
.app-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-width: 100vw;
  min-height: 100vh;
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
  min-width: 50vw;
  min-height: 50vh;
  /* margin-top: 40vh; */
  text-align: center;
}

h1 {
  font-family: Arial, Helvetica, sans-serif;
  text-transform: lowercase;
}

footer {
  text-align: center;
}

.link {
  color: white;
}

.link:visited {
  color: grey;
}

.link:hover {
  color: blueviolet;
}
</style>
