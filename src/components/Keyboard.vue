<script setup lang="ts">
import Keyboard from "simple-keyboard";
import "simple-keyboard/build/css/index.css";
import { onMounted, ref, watch } from "vue";
const props = defineProps<{
  guessedLetters: {
    error: string[];
    success: string[];
    warn: string[];
  },
  isDark:boolean;
}>();

const keyboard = ref<Keyboard | null>(null);
const emits = defineEmits(["onKeyPress"]);
const onKeyPress = (key) => {
  emits("onKeyPress", key);
};
onMounted(() => {
  keyboard.value = new Keyboard("simple-keyboard", {
    layout: {
      default: [
        "q w e r t y u i o p",
        "a s d f g h j k l",
        "{bksp} z x c v b n m {enter}",
      ],
    },
    onKeyPress,
  });
});
watch(
  () => props.guessedLetters,
  (cur, prev) => {
    keyboard.value?.addButtonTheme(cur.error.join(" "), "error");
    keyboard.value?.addButtonTheme(cur.success.join(" "), "success");
    keyboard.value?.addButtonTheme(cur.warn.join(" "), "warn");
  },
  { deep: true }
);
</script>

<template>
  <div class="simple-keyboard"></div>
</template>

<style>
div.error {
  @apply bg-red-400 !important;
  @apply text-white !important;
}

div.success {
  @apply bg-green-600 !important;
  @apply text-white !important;
}

div.warn:not(.success) {
  @apply bg-orange-300 !important;
  @apply text-white !important;
}
</style>
