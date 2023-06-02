<template>
	<div :class="`info ${showInfo ? '' : 'info-hide'}`">
		<p>
			You will get more info by hovering over these icons on cards:
			<span
				v-for="(item, index) in info"
				:key="index"
				v-html="item.icon" />
		</p>
		<button @click="toggle">
			<span
				v-if="!showInfo"
				v-html="infoIcon"></span>
			<span v-if="showInfo">Hide</span>
		</button>
	</div>
</template>

<script setup>
import { ref } from 'vue'
const infoIcon = '&#128172;'
const showInfo = ref(true)
const toggle = () => {
	showInfo.value = !showInfo.value
}

const info = [
	{
		icon: '&#128004;',
	},
	{
		icon: '&#127866;',
	},
]
</script>

<style scoped>
.info {
	background-color: azure;
	padding: 10px;
	border-radius: 10px;
	border: 1px solid #000;
	margin-bottom: 40px;
	width: fit-content;
}

.info.info-hide {
	background-color: transparent;
	padding: 0;
	border-radius: none;
	border: none;
}

.info.info-hide p {
	display: none;
}

.info.info-hide button {
	background-color: transparent;
	font-size: 30px;
}

.info p {
	font-size: 20px;
	margin: 0 0 10px 0;
}

.info p span {
	font-size: 30px;
	margin-right: 10px;
}

.info button {
	border: none;
	background-color: cadetblue;
	color: #fff;
	font-size: 16px;
	padding: 5px 10px;
	border-radius: 10px;
	cursor: pointer;
}
</style>
