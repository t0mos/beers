<template>
	<Loading v-if="loading" />
	<div v-if="res">
		<Info />
		<div class="sort">
			<label for="sort">Sort:</label>
			<select
				name="sort"
				id="sort"
				v-model="sortOrder">
				<option value="asc">Sort by lowest ABV</option>
				<option value="desc">Sort by highest ABV</option>
			</select>
			<button @click="toggleHideCentennial">
				<span v-if="hideCentennial">Show</span>
				<span v-else>Hide</span>
				Centennial
			</button>
		</div>

		<h1>Total items: {{ sortedAndFilteredBeers.length }}</h1>

		<div class="card-wrapper">
			<Card
				v-for="item in sortedAndFilteredBeers"
				:key="item.id"
				:name="item.name"
				:image="item.image_url"
				:abv="item.abv"
				:ibu="item.ibu"
				:tagline="item.tagline"
				:description="item.description"
				:ingredients="item.ingredients.hops" />
		</div>
	</div>
	<Error
		v-if="err"
		:error="err" />
</template>

<script setup>
import Card from './components/Card.vue'
import Loading from './components/Loading.vue'
import Error from './components/Error.vue'
import Info from './components/Info.vue'

import { onMounted, ref, computed } from 'vue'

const url = 'https://api.punkapi.com/v2/beers?brewed_after=11-2012'
const res = ref(null)
const loading = ref(true)
const err = ref(null)
const sortOrder = ref('asc')
const hideCentennial = ref(false)

async function fetchBeers() {
	try {
		const response = await fetch(url)
		if (!response.ok) {
			throw new Error('Request failed with status ' + response.status)
		}
		const data = await response.json()
		res.value = data
	} catch (error) {
		err.value = error.message
	}
	loading.value = false
}

const sortedAndFilteredBeers = computed(() => {
	const beers = res.value
	if (beers && beers.length) {
		let filteredBeers = beers
		if (hideCentennial.value) {
			filteredBeers = filteredBeers.filter((item) => {
				const hops = item.ingredients.hops || []
				return !hops.some((hop) => hop.name === 'Centennial')
			})
		}

		const order = sortOrder.value === 'asc' ? 1 : -1
		return filteredBeers.slice().sort((a, b) => (a.abv - b.abv) * order)
	}
	return beers
})

const toggleHideCentennial = () => {
	hideCentennial.value = !hideCentennial.value
}

onMounted(() => {
	fetchBeers()
})
</script>

<style>
#app {
	margin: 40px 80px;
}

.card-wrapper {
	display: flex;
	flex-wrap: wrap;
	justify-content: center;
	gap: 20px;
}

.sort {
	margin-bottom: 20px;
}

.sort label {
	position: absolute;
	top: -99999px;
	left: -99999px;
	visibility: hidden;
}

.sort select {
	font-size: 18px;
	padding: 3px 5px;
	border-radius: 10px;
}

.sort button {
	font-size: 18px;
	padding: 5px;
	border-radius: 10px;
	margin-left: 10px;
	border: 0.5px solid;
	background-color: #fff;
	cursor: pointer;
}

@media only screen and (max-width: 768px) {
	#app {
		margin: 20px 30px;
	}
}

@media only screen and (max-width: 426px) {
	.sort button {
		margin-top: 20px;
		margin-left: 0;
	}

	.sort select,
	.sort button {
		width: 100%;
		padding: 10px;
	}
}
</style>
