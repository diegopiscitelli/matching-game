<template>
	<div
	class="card"
	:class="{ 'card--flipped': props.cardData.isFlipped, 'card--matched': props.cardData.matched }"
	@click="$emit('cardClick', props.cardData.uuId, props.cardData.id)"
	>
		<div class="card__inner">
			<div class="card__front"><div>?</div></div>
			<div class="card__back" :style="{ backgroundImage: `url(${props.cardData.url})` }"></div>
		</div>
	</div>
</template>

<script setup>

import { defineProps } from 'vue';
const props = defineProps({ cardData: Object });

</script>

<style scoped lang="scss">

.card--matched, .card--flipped {
	pointer-events: none;
}
.card {
	width: 12%;
	height: 32%;
	border-radius: 5%;
	display: flex;
	background-color: transparent;
	overflow: hidden;
	cursor: pointer;
	
	&.card--flipped .card__inner {
		transform: rotateY(180deg);
	}
	.card__inner{
		width: 100%;
		height: 100%;
		text-align: center;
		transition: transform 0.6s;
		transform-style: preserve-3d;

		div{
			position: absolute;
			width: 100%;
			height: 100%;
			-webkit-backface-visibility: hidden;
			backface-visibility: hidden;

		}
		.card__front{
			background-color: rgb(241, 245, 243);
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			user-select: none;
			div{
				position: relative;
				height: auto;
				font-weight: 900;
				font-size: 80px;
				color: #d9e2e7;
			}
		}
		.card__back{
			transform: rotateY(180deg);
			background-size:cover;
			background-position: center;
		}
	}
	// MediaQueries
	@media screen and (max-width: 900px){
		width: 15%;
		height: 22%;
	}
	@media screen and (max-width: 600px){
		width: 23%;
		height: 15%;
		.card__inner{
			.card__front{
				div{ font-size: 50px; }
			}
		}
	}
}
</style>