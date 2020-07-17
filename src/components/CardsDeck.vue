<template>
	<div>
		<button class="btn btn-primary" v-on:click="generateDeck()">Générer un deck</button>
		<card
		v-for="(card, index) in cards"
		:key="index"
		:value="card.value"
		:family="card.family"
		></card>
	</div>
</template>

<script>
	import Card from './Card'

	export default {
		name: 'CardsDeck',
		components: {
						'card': Card,
		},
		data() {
			return{
				cardsNumber: 52,
				cards: []
			}
		},
		methods: {
			generateDeck(){
				let tempFamily = '';
				for (let i = 0; i < 4; i++){
					switch (i){
						case 0 : tempFamily = "coeur"; break;
						case 1 : tempFamily = "pique"; break;
						case 2 : tempFamily = "trèfle"; break;
						case 3 : tempFamily = "carreau"; break;
					}
					for (let cardValue = 1; cardValue <= 13; cardValue++){
						this.cards.push({value: cardValue, family: tempFamily});
					}
				}
			}
		}
	}
</script>

<style>

</style>