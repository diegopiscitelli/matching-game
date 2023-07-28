<template>
	<ModalComponent :modalOn="playerWin" :userName="props.userName" @modalClick="handleModalClick"></ModalComponent>
  <div class="game__container">
		<nav class="nav">
			<h1 class="nav__element">Memory</h1>
			<div class="result nav__element">
				<div class="result__inner">
					<div>Aciertos:<span class="result__hits">{{ result.hit }}</span></div>
					<div>Errores:<span class="result__misses">{{ result.miss }}</span></div>
				</div>
			</div>
			<div class="nav__element nav_button_container">
				<ButtonComponent @buttonClick="resetGame">
					<img src="../assets/restart_icon.svg"/>
				</ButtonComponent>
			</div>
		</nav>
    <div class="cards" v-if="cards.length">
      <CardComponent  v-for="card in cards" :key="card.uuid" :cardData="card" @cardClick="handleCardClick"/>
    </div>
  </div>
</template>

<script setup>

import  { ref, onMounted, watch, computed, nextTick } from 'vue'
import CardComponent from './CardComponent.vue'
import ButtonComponent from './ButtonComponent.vue'
import ModalComponent from './ModalComponent.vue'
import { uuid } from 'vue-uuid'; 
import { defineProps } from 'vue';

const props = defineProps({ userName: String });

let serverImages = [];
let cards = ref([]);
let isPlaying = ref(0);
let comparingNow = ref([]);
let result = ref({
	hit: 0,
	miss: 0
});

onMounted(() => {
  getImages();
});

const playerWin = computed(() => {
	let numberForAWin = cards.value.length / 2;
	let winner
	if (result.value.hit == numberForAWin && result.value.hit > 0){
		winner = true
	} else{
		winner = false
	}
	return winner
})

watch(isPlaying, () => {
	// Is a hit
	if (comparingNow.value.length > 1 && comparingNow.value[0] === comparingNow.value[1]) {
		updateCardsStatus(true, comparingNow.value);
		result.value.hit ++
	}
	// Is a miss
	else if (comparingNow.value.length > 1) {
		updateCardsStatus(false, comparingNow.value);
		result.value.miss ++
	}
});

function handleModalClick() {
	resetGame()
}

function handleCardClick(uuid, id) {
	const found = cards.value.find(element => element.uuId == uuid);
	if(comparingNow.value.length < 2) {
		isPlaying.value ++
		found.isFlipped = true;
		comparingNow.value.push(id);
	}
}

function resetComparingNow() {
	comparingNow.value = [];
}

function updateCardsStatus(isAWin, ids) {
	let cardsToFlip = ids;
	if(!isAWin){
		cardsToFlip.forEach((id) => {
			setTimeout(()=>{
				for (let object of cards.value) {
					if (object.id === id) {
						object.isFlipped = false;
					}
				}
				resetComparingNow();
			}, 1200)
		})
	} else{
		cardsToFlip.forEach((id) => {
			let found = cards.value.find(element => element.id == id);
			found.matched = true;
			resetComparingNow();
		})
	}
}
const getImages = async () => {
	// this.isLoadingData = true;
	let status;
	const res = fetch(' https://fed-team.modyo.cloud/api/content/spaces/animals/types/game/entries?per_page=12', {
		method: 'GET',
		cache: 'default'
	})
	.then( (res) => {
		status = res.status;
		return res.json()
	})
	.then( (json) => {
		return json;
	})

	let data = await res;

	if(status === 200 ){
		const formattedData = data.entries.map( (element) => { 
			let imageData = element.fields.image;
			let imgDataFormated = {
				id: imageData.uuid,
				uuId:'',
				url: imageData.url,
				title: imageData.title,
				isFlipped: false,
				matched: false
			}
			return imgDataFormated
		});
		serverImages = formattedData;
		generateCards();
	}
}

const generateCards = () => {
	if (cards.value.length){
		cards.value = [];
	}
	let finalArray = [];
	let duplicatedArrayOfImages = [...serverImages, ...serverImages];
	duplicatedArrayOfImages.forEach((e) => {
		function newObject (){
			return{
				...e,
				uuId: uuid.v1()
			}
		}
		e = newObject();
		finalArray.push(e);
	})
	const randomizedArray = shuffle(finalArray);
	cards.value = randomizedArray;
};

const shuffle = (array) => { 
  for (let i = array.length - 1; i > 0; i--) { 
    const j = Math.floor(Math.random() * (i + 1)); 
    [array[i], array[j]] = [array[j], array[i]]; 
  } 
  return array; 
}; 

function resetGame() {
	for (let object of cards.value) {
		if (object.isFlipped) {
			object.isFlipped = false;
		}
	}
	nextTick(() => {
		resetResult();
		generateCards();
	})
	
}

function resetResult() {
	result.value = {
		hit: 0,
		miss: 0
	}
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.game__container {
	display: flex;
	width: 80%;
	height: 100vh;
	flex-direction: column;
	justify-self: center;
	padding: 10px 0px;
	box-sizing: border-box;
}

nav{
	display: flex;
	align-items: center;
	justify-content: space-between;
	padding: 15px 0px;
	.nav__element{
		width: 30%;
	}
	h1{
		width: 100%;
		text-align: left;
		font-size: 20px;
		line-height: 20px;
	}
	.result{
		display: flex;
		justify-content: center;
		font-weight: 700;
		.result__inner{
			display: flex;
			border-radius: 5px;
			align-items: center;
			div:not(:last-child){
				margin-right: 15px;
			}
			span{
				display: inline-block;
				width: 35px;
				padding: 5px 10px;
				background-color: #ffffff;
				border-radius: 5px;
				margin-left: 5px;
				font-size: 24px;
				font-weight: 800;
				text-align: center;
				box-shadow: 2px 1px 4px 0px #dfdfdf;
			}
			.result__hits{
				color: #46E369;
			}
			.result__misses{
				color: #FB6363;
			}
		}
	}
	.nav_button_container{
		display: flex;
		justify-content: flex-end;
	}
	button{
		justify-self: flex-end;
		font-size: 12px;
		img{
			position: relative;
			top: 2px;
			width: 25px;
			height: 25px;
			transform-origin: 48%;
			transition: transform 0.3s;
		}
		&:hover img{
			transform: rotateZ(-320deg);
		}
	}
	@media screen and (max-width: 700px){
		
	}
}
.cards{
	display: flex;
	width: 100%;
	height: 90%;
	flex-wrap: wrap;
	justify-self: center;
	justify-content: space-around;
}
@media screen and (max-width: 800px){
	.game__container {
		display: flex;
		width: 90%;
		height: 100vh;
		flex-direction: column;
		justify-self: center;
		padding: 10px 0px;
		box-sizing: border-box;
	}
	nav{
		width: 100%;
		text-align: center;
		padding-top: 25px;
		.nav__element{
			width: auto;
		}
		h1{
			position: absolute;
			top: 0px;
			text-align: center;
		}
		.result{
			width: 90%;
			justify-content: flex-start;
			.result__inner{
				span{
					width: 25px;
					font-size: 18px;
				}
			}
		}
		button{
		font-size: 12px;
		min-height: 30px;

		img{
			width: 20px;
			height: 20px;
		}}
	}
}
</style>
