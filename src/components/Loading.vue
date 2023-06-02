<template>
	<div>
		<span
			v-html="loading"
			class="loading" />
	</div>
</template>

<script setup>
const loading = 'loading... &#127867;'
</script>

<style scoped>
.loading {
	display: block;
	text-align: center;
	font-size: 50px;
}
</style>
