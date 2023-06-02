<template>
	<div class="error"><span v-html="icon" /> {{ props.error }}</div>
</template>

<script setup>
import { defineProps } from 'vue'

const icon = '&#128683;'
const props = defineProps(['error'])
</script>

<style scoped>
.error {
	display: block;
	text-align: center;
	font-size: 50px;
}
</style>
