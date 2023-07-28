<template>
	<transition >
	<div v-show="props.modalOn" class="modal">
		<transition name="scale">
			<div v-show="props.modalOn" class="modal__inner">
				<h1>¡Felicidades {{ props.userName }}!</h1>
				<div><h2>Encontraste todas las parejas</h2>
				<img alt="TROPHY" src="../assets/modal_img.svg"/></div>
				<ButtonComponent @buttonClick="$emit('modalClick')">Volver a jugar</ButtonComponent>
			</div>
		</transition>
	</div>
	</transition>
</template>

<script setup>

import { defineProps } from 'vue';
import ButtonComponent from './ButtonComponent.vue'
const props = defineProps({ modalOn: Boolean, userName: String });

</script>

<style scoped lang="scss">
// Modal Animation
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}
.v-enter-from,
.v-leave-to {
  opacity: 0;
}
.modal {
	box-sizing: border-box;
	position: fixed;
	z-index: 1;
	display: flex;
	align-items: center;
	justify-content: center;
	width: 100%;
	height: 100vh;
	background-color: rgba($color: #504359, $alpha: 0.5);
	padding: 25px;
	.modal__inner{
		min-width: 30%;
		min-height: 30%;
		max-height: 70vh;
		padding: 20px;
		background-color: #fff;
		border-radius: 15px;
		transform-origin: 50%;
		
		img{
			opacity: 0.7;
			max-height: 150px;
			margin-bottom: 10px;
		}
	}
// Modal__inner Animation
.scale-enter-active,
.scale-leave-active {
  transition: scale 0.5s ease;
}
.scale-enter-from,
.scale-leave-to {
  scale: 0;
}

}

</style>