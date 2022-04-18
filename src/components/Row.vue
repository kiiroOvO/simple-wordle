<script setup lang="ts">
import Box from './Box.vue';
defineProps<{
  value: string
  solution: string,
  submitted: boolean,
}>()
</script>
<template>
  <div class="grid max-w-xs grid-cols-5 gap-1 mx-auto mb-1">
    <template v-for="i in 5" :key="i">
      <Box :letter="value[i - 1]"></Box>
    </template>
  </div>
</template>
<style scoped>
</style>