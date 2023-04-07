<template>
  <div class="table-wrapper" v-if="text">
    <table>
      <tr>
        <th>Characters</th>
        <td
          align="center"
          valign="center"
          class="center-text"
          v-for="(character, j) in characters"
          :key="j"
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
          v-for="(decimal, j) in decimalCodes"
          :key="j"
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
          v-for="(hexCode, j) in hexCodes"
          :key="j"
        >
          {{ hexCode }}
        </td>
      </tr>
    </table>
  </div>
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

.table-wrapper {
  border: 2px solid white;
  border-radius: 10px;
  padding: 20px;
  margin: 15px;
  font-family: monospace;
}

table {
  border-collapse: collapse;
}

td:not(:last-child)  {
  border-right: 2px solid white;
}

td {
  margin: 5px;
  padding: 5px;
}
</style>
