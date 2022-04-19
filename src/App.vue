<script setup lang="ts">
import { computed, onMounted, reactive } from "vue";
import Keyboard from "./components/Keyboard.vue";
import Row from "./components/Row.vue";
import Toggle from "./components/Toggle.vue";
import { useDark, useToggle } from "@vueuse/core";

const isDark = useDark();
const toggleDark = useToggle(isDark);
const isWin = computed(
  () => state.guesses[state.currentIndex - 1] === state.solution
);
const handleKeyPress = (key) => {
  if (state.currentIndex >= 6 || isWin.value) return;
  const currentGuess = state.guesses[state.currentIndex];
  if (key === "{enter}") {
    if (currentGuess.length === 5) {
      state.currentIndex++;
    }
    for (let i = 0; i < currentGuess.length; i++) {
      let c = currentGuess.charAt(i);
      if (c === state.solution.charAt(i)) {
        // 找到了
        state.guessedLetters.success.push(c);
      } else if (state.solution.indexOf(c) !== -1) {
        //位置错误
        state.guessedLetters.warn.push(c);
      } else {
        state.guessedLetters.error.push(c);
      }
    }
  } else if (key === "{bksp}") {
    state.guesses[state.currentIndex] = currentGuess.slice(0, -1);
  } else if (currentGuess.length < 5) {
    if (/[a-zA-Z]/.test(key)) {
      state.guesses[state.currentIndex] += key;
    }
  }
};
const state = reactive({
  solution: "chill",
  guesses: ["", "", "", "", "", ""],
  currentIndex: 0,
  guessedLetters: {
    error: [] as string[],
    success: [] as string[],
    warn: [] as string[],
  },
});
onMounted(() => {
  window.addEventListener("keydown", (e) => {
    if (e.metaKey || e.shiftKey || e.ctrlKey) return;

    e.preventDefault();
    let key =
      e.key === "Enter"
        ? "{enter}"
        : e.key === "Backspace"
        ? "{bksp}"
        : e.key.toLocaleLowerCase();
    handleKeyPress(key);
  });
});
</script>

<template>
  <main class="text-black dark:text-white">
    <div class="flex h-screen flex-col max-w-lg mx-auto justify-evenly">
      <div class="self-end mx-4 my-1">
        <Toggle :isDark="isDark" :toggleDark="toggleDark"></Toggle>
      </div>
      <div>
        <Row
          v-for="(guess, i) in state.guesses"
          :key="i"
          :value="guess"
          :solution="state.solution"
          :submitted="i < state.currentIndex"
        ></Row>
      </div>
      <div v-if="isWin" class="text-center">You Won 🏆.</div>
      <Keyboard
        @on-key-press="handleKeyPress"
        :guessedLetters="state.guessedLetters"
        :isDark="isDark"
      />
    </div>
  </main>
</template>
