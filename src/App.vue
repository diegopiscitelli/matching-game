<template>
	<UserForm v-show="!userName.length" @formCompleted="handleFormCompleted"/>
	<GameComponent :userName="userName"/>
</template>

<script setup>
import  { ref, onBeforeMount} from 'vue'
import UserForm from './components/UserForm.vue';
import GameComponent from './components/GameComponent.vue'

const userName = ref('');

onBeforeMount(() => {
  let localStoragedName = localStorage.getItem('userName');
	if(localStoragedName){
		userName.value = localStoragedName;
	}
});
function handleFormCompleted(input) {
	userName.value = input;
	localStorage.setItem('userName', input)
}

</script>

<style lang="scss">
body{
	margin: 0;
}
h1, h2{
	margin: 10px 0;
}
#app {
	font-family: 'Poppins', sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #504359;
	display: flex;
	justify-content: center;
}
</style>
