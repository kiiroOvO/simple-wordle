<script setup lang="ts">
import { ref, watch } from "vue";
import Box from "./Box.vue";
const props = withDefaults(
  defineProps<{
    value: string;
    solution: string;
    submitted: boolean;
  }>(),
  {
    value: "",
  }
);
const colors = new Array(5).fill("");
watch(
  () => props.submitted,
  async (cur, prev) => {
    if (props.submitted) {
      let s = props.solution;
      let v = props.value;
      const arr = new Array(5).fill("red");
      // 存放 red 和 yellow
      const letterPool: string[] = [];
      for (let i = 0; i < 5; i++) {
        if (s.charAt(i) === v.charAt(i)) {
          arr[i] = "green";
        } else {
          letterPool.push(s.charAt(i));
        }
      }
      // 处理 red 和 yellow
      arr.forEach((color, index) => {
        if (color === "red") {
          if (letterPool.indexOf(v.charAt(index)) !== -1) {
            //位置不对
            //remove
            letterPool.splice(letterPool.indexOf(v.charAt(index)), 1);
            arr[index] = "yellow";
          }
        }
        colors[index] = arr[index];
      });
      await new Promise((resolve) => setTimeout(resolve, 500));
    }
  }
);
</script>
<template>
  <div class="grid max-w-xs grid-cols-5 gap-1 mx-auto mb-1">
    <template v-for="i in 5" :key="i">
      <Box
        :color="colors[i - 1]"
        :letter="value[i - 1] ? value[i - 1] : ''"
      ></Box>
    </template>
  </div>
</template>
<style scoped></style>
