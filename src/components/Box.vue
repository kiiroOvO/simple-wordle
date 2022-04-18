<script setup lang="ts">
withDefaults(
  defineProps<{
    letter: string;
    color: string;
  }>(),
  {}
);
</script>

<template>
  <div
    class="flex col-span-1 justify-center items-center h-16 uppercase border-2 border-gray-200 transition-all duration-300"
    :class="{
      'bg-gray-500 text-white': color === 'gray',
      'bg-green-500 text-white': color === 'green',
      'bg-red-500 text-white': color === 'red',
      'bg-yellow-500 text-white': color === 'yellow'
    }">
    {{ letter }}
  </div>
</template>
