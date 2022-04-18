
<script setup lang="ts">
withDefaults(defineProps<{
  letter?: string
}>(), {
  letter: ""
})
</script>

<template>
  <div class="flex col-span-1 justify-center items-center h-16 uppercase border-2 border-gray-200">
    {{ letter }}
  </div>
</template>