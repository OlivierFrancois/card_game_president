<template>
	<div>
		<button class="btn btn-primary mr-3 my-1" v-on:click="generateDeck()">Générer un deck</button>
		<button class="btn btn-primary mr-3 my-1" v-on:click="shuffleDeck()">Mélanger le deck</button>
		<button class="btn btn-primary mr-3 my-1 " v-on:click="displayDeck = !displayDeck">Voir/Masquer le deck</button> <br>
		<button class="btn btn-primary mr-3 my-1 " v-on:click="distrib()">Distribuer</button>
		<button class="btn btn-danger mr-3  my-1" v-on:click="deleteDeck()">Vider le deck</button>
		
		
		<div class="deck">
			<div class="deck-body" v-if="displayDeck">
				<card
				v-for="(card, index) in cards"
				:key="index"
				:card="card"
				></card>
			</div>
		</div>
		<br>
		<div
		class="hand"
		v-for="(hand, index) in hands"
		:key="index">
			<div class="hand-body">
				<card
				v-for="(card, index) in hand"
				:key="index"
				:card="card"
				></card>
			</div>
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
				cards: [],
				displayDeck: false,
				playerNb: 4,
				hands: []
			}
		},
		methods: {
			generateDeck(){
				this.cards = []; //reset the deck
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

				this.displayDeck = true;
			},
			deleteDeck(){
				this.cards = [];
			},
			shuffleDeck(){
				let tempArr = this.cards.slice(); //clone the deck
				this.cards = [];

				for (let i = 0; i < this.cardsNumber; i++){
					let rngIndex = Math.floor(Math.random() * Math.floor(tempArr.length)); //We take a random card from tempArr
					//console.log(rngIndex);
					this.cards.push(tempArr[rngIndex]); //We add it to our deck
					tempArr.splice(rngIndex, 1);
				}

				//Refresh the array
				this.cards.push({value: -1, family: ''});
				this.cards.pop();
			},
			distrib(){
				for (let i = 0; i < this.playerNb; i++) {
					this.hands.push([]);
				}
				
				let currentHand = 0;	
				for (let i = 0; i < this.cardsNumber; i++) {
					this.hands[currentHand].push(this.cards[0]);
					
					this.cards.shift();

					currentHand++;
					if (currentHand >= this.playerNb){
						currentHand = 0;
					}
				}
			}
		}
	}
</script>

<style>
	.deck, .hand{
		width: 90%;
		margin: auto;
		margin-top: 5px;
	}
	
	.deck-body, .hand-body{
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		background: #333;
		padding: 1%;
		margin: 0!important;
	}
</style>