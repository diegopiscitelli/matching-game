<template>
	<transition>
		<div class="user_form">
			<div class="user_form__inner">
				<h1>¿Cómo te llamas?</h1>
				<form v-show="true">
					<input
					v-model.lazy="name"
					type="text"
					placeholder="Tu nombre"
					maxlength="10"
					/>
				</form>
				<ButtonComponent @buttonClick="$emit('formCompleted', name)">Continuar</ButtonComponent>
			</div>
		</div>
	</transition>
	
</template>

<script setup>

import { ref } from 'vue';
import ButtonComponent from './ButtonComponent.vue'

const name = ref('');

</script>

<style scoped lang="scss">
.v-enter-active,
.v-leave-active {
  transition: all 0.5s ease-in-out;
}
.v-enter-from,
.v-leave-to {
  transform: translateY(-100vh);
}
.user_form {
	position: fixed;
	z-index: 1;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	width: 100%;
	height: 100vh;
	background-color: #ffffff;
	.user_form__inner{
		flex-direction: column;
    display: flex;
		max-width: 300px;
		min-height: 30%;
		max-height: 70vh;
	}
	h1{
		font-weight: 300;
	}
	form{
		border-radius: 15px;
		transform-origin: 50%;
		input {
		font-family: 'Poppins', sans-serif;
		box-sizing: border-box;
		width: 100%;
		margin: 5px 0;
		padding: 10px;
		border: solid 1.5px #D9D9D9;
		background-color: #D9D9D9;
		outline: none;
		color: #272727;
		font-weight: 500;
		border-radius: 5px;
		transition: all 0.3s;
		&:focus{
			background-color: #fff;
		}
		}
		input::placeholder {
			color: #969696;
		}
		
		button{
			position: relative;
			width: 100%;
		}
	}
	
	.fade-enter-active,
	.fade-leave-active {
		transition: scale 0.5s ease;
	}
	.fade-enter-from,
	.fade-leave-to {
		scale: 0;
	}
}

</style>