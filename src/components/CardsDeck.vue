<template>
	<div>
		<button class="btn btn-primary mr-3" v-on:click="generateDeck()">Générer un deck</button>
		<button class="btn btn-primary mr-3" v-on:click="shuffleDeck()">Mélanger le deck</button>
		<button class="btn btn-primary" v-on:click="deleteDeck()">Vider le deck</button>
		
		<div class="deck">
			<card
			v-for="(card, index) in cards"
			:key="index"
			:card="card"
			></card>
		</div>
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
			},
			deleteDeck(){
				this.cards = [];
			},
			shuffleDeck(){
				let tempArr = this.cards.slice(); //clone the deck

				for (let i = 0; i < this.cardsNumber; i++){
					let rngIndex = Math.floor(Math.random() * Math.floor(tempArr.length)); //We take a random card from tempArr
					//console.log(rngIndex);
					this.cards[i] = tempArr[rngIndex]; //We add it to our deck
					tempArr.slice(rngIndex, 1);
				}

				//Refresh the array
				this.cards.push({value: -1, family: ''});
				this.cards.pop();
			}
		}
	}
</script>

<style>
	.deck{
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		width: 80%;
		margin: auto;
	}
</style>