<template>
  <input :modelValue="modelValue" @input="onInput" />
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  props: {
    modelValue: String,
  },
  emits: ["update:modelValue"],
  setup(props, { emit }) {
    function onInput(value: InputEvent) {
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

<style scoped></style>
