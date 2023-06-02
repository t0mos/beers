<template>
	<div class="card">
		<div class="card__img">
			<img
				:src="props.image"
				:alt="props.name" />
		</div>
		<div class="card__info">
			<div class="card__info-extra">
				<span>ABV: {{ props.abv }}</span>
				<span>IBU: {{ props.ibu }}</span>
			</div>
			<h2 class="card__info-name">{{ props.name }}</h2>
			<p class="card__info-tagline">{{ props.tagline }}</p>
			<p class="card__info-description">{{ props.description }}</p>
			<div
				v-for="(item, index) in props.ingredients"
				:key="index"
				class="card__ingredients">
				<span
					v-show="item.name === 'Lactose'"
					class="card__ingredients--lactose">
					&#128004; <span>{{ lactose }}</span>
				</span>
				<span
					v-show="item.add === 'dry hop'"
					class="card__ingredients--dry-hopped">
					&#127866; <span>{{ item.name }}: {{ dryHopped }}</span>
				</span>
			</div>
		</div>
	</div>
</template>

<script setup>
import { defineProps } from 'vue'

const lactose = 'Contains lactose'
const dryHopped = 'Dry hopped'

const props = defineProps([
	'name',
	'image',
	'abv',
	'ibu',
	'tagline',
	'description',
	'ingredients',
])
</script>

<style scoped>
.card {
	display: flex;
	justify-content: center;
	align-items: center;
	width: 25%;
	border-radius: 20px;
	padding: 40px;
	background-color: #ffffff;
	position: relative;
	box-shadow: 0px 6px 35px -24px #000000;
	transition: all 0.3s;
}

.card:hover {
	box-shadow: none;
	background-color: #f6f6f6;
}

.card:hover img {
	transform: rotate(-10deg);
}

.card__img {
	width: auto;
	height: 200px;
	margin-right: 20px;
}

.card__img img {
	width: 100%;
	height: 100%;
	object-fit: contain;
	transition: all 0.3s;
}

.card__info {
	width: 80%;
}

.card__info-extra {
	position: absolute;
	top: 20px;
	left: 20px;
}

.card__info-extra > span {
	padding: 5px 10px;
	border-radius: 20px;
}

.card__info-extra > span:first-child,
.card__info-extra > span:nth-child(2) {
	margin-right: 20px;
}

.card__info-extra > span:first-child {
	background-color: #ddfefe;
}

.card__info-extra > span:nth-child(2) {
	background-color: #ede4fa;
}

.card__info-name {
	font-size: 22px;
}

.card__info-tagline {
	font-style: italic;
}
.card__info-tagline::before,
.card__info-tagline::after {
	content: '"';
}

.card__info-description {
	font-size: 18px;
}

.card__ingredients {
	display: inline-block;
}

.card__ingredients--lactose,
.card__ingredients--dry-hopped {
	cursor: pointer;
	position: relative;
	font-size: 24px;
}

.card__ingredients--lactose span,
.card__ingredients--dry-hopped span {
	display: none;
	position: absolute;
	top: -21px;
	left: 10px;
	background-color: #ff9d00;
	width: max-content;
	padding: 3px 5px;
	border-radius: 20px;
	z-index: 1;
	font-size: 14px;
}

.card__ingredients--lactose:hover span,
.card__ingredients--dry-hopped:hover span {
	display: block;
}

@media only screen and (max-width: 1300px) {
	.card {
		width: 40%;
	}
}

@media only screen and (max-width: 1075px) {
	.card {
		width: 100%;
	}
}

@media only screen and (max-width: 425px) {
	.card {
		padding: 60px 20px 20px 20px;
		flex-direction: column;
	}
}
</style>
