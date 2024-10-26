<script setup lang="ts">
defineProps<{ 
	flowers: any;
}>()
</script>

<template>
	<li v-for="flower in flowers" :key="flower">{{ flower }}</li>
</template>
