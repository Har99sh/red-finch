<script setup>
// import type { Task } from "@/types"
// import {
//     Card,
//     CardTitle,
//     CardHeader,
//     Avatar,
//     CardSubtitle
// } from "@progress/kendo-vue-buttons"
const props = defineProps({
  task: Object,
});
</script>

<template>
  <div class="p-5 bg-white">
    {{ task }}
  </div>
</template>
