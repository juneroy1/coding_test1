<template>
<span class="text-gray-500"> {{ formattedDate}}</span>
</template>

<script setup>
import { computed } from 'vue';
import { format, parseISO } from 'date-fns';
const props = defineProps({
  dateTime: String,
});

const formattedDate = computed(() => {
  const date = parseISO(props.dateTime);
  return format(date, 'EEEE, MMMM d, yyyy p');
});
</script>