<template>
  <input type="text" :modelValue="modelValue" @input="onInput" placeholder="Input Text Here" />
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  props: {
    modelValue: String,
  },
  emits: ["update:modelValue"],
  setup(props, { emit }) {
    function onInput(value: Event) {
      if (!value?.target) return;
      const inputEl = <HTMLInputElement | null>value.target;
      console.log("Input things....");
      emit("update:modelValue", inputEl?.value);
    }

    return {
      onInput,
    };
  },
});
</script>

<style scoped>

input[type=text] {
  border-radius: 20px;
  background-color: transparent;
  border: 2px solid white;
  padding: 10px;
  margin: 10px;
  color: white;
}

::selection {
  background-color: white;
  color: darkblue;
}

::placeholder {
  color: white;
}
</style>
