<template>
	<button @click="$emit('buttonClick')">
		<slot>Play Again</slot>
	</button>
</template>

<script>
export default {

}
</script>

<style scoped lang="scss">
button{
	color: #372f3c;
	font-family: 'Poppins', sans-serif;
	border: none;
	background-color: #F7C62B;
	font-size: 13px;
	text-align: center;
	border-radius: 5px;
	cursor: pointer;
	padding: 5px 10px;
	min-height: 45px;
	border: 2px solid transparent;
	transition: all 0.3s;
	&:hover {
	border-color: #504359;
  }
}

</style>