<template>
  <table v-if="text">
    <tr>
      <th>Characters</th>
      <td
        align="center"
        valign="center"
        class="center-text"
        v-for="character in characters"
        :key="character"
      >
        {{ character }}
      </td>
    </tr>
    <tr>
      <th>Decimal</th>
      <td
        align="center"
        valign="center"
        class="center-text"
        v-for="decimal in decimalCodes"
        :key="decimal"
      >
        {{ decimal }}
      </td>
    </tr>
    <tr>
      <th>Hex</th>
      <td
        align="center"
        valign="center"
        class="center-text"
        v-for="hexCode in hexCodes"
        :key="hexCode"
      >
        {{ hexCode }}
      </td>
    </tr>
  </table>
</template>

<script lang="ts">
import { computed, ComputedRef, defineComponent } from "vue";
import _ from "lodash";

export default defineComponent({
  props: {
    text: String,
  },
  setup(props) {
    const characters = computed(() => {
      if (!props.text) return [];
      const goodText = props.text;
      const length = goodText.length;
      return _.split(goodText, "");
    });
    const decimalCodes: ComputedRef<string[]> = computed(() => {
      if (!props.text) return [];
      const goodText = props.text;

      return _.split(goodText, "").map((unicodeCharacter) => {
        return _.range(0, unicodeCharacter.length)
          .map((index) => unicodeCharacter.charCodeAt(index))
          .join(", ");
      });
    });
    const hexCodes: ComputedRef<string[]> = computed(() => {
      if (!props.text) return [];
      const goodText = props.text;

      return _.split(goodText, "").map((unicodeCharacter) => {
        return _.range(0, unicodeCharacter.length)
          .map((index) => unicodeCharacter.charCodeAt(index).toString(16))
          .join(", ");
      });
    });

    return {
      characters,
      decimalCodes,
      hexCodes,
    };
  },
});
</script>

<style scoped>
.center-text {
  text-align: center;
  vertical-align: middle;
}
</style>
