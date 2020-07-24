<template>
	<div>
		<button class="btn btn-primary mr-3 my-1" v-on:click="generateDeck()">Générer un deck</button>
		<button class="btn btn-primary mr-3 my-1" v-on:click="shuffleDeck()">Mélanger le deck</button>
		<button class="btn btn-primary mr-3 my-1 " v-on:click="distrib()">Distribuer</button>
		
		<div class="deck" v-if="cardsDeck.length > 0">
			<div class="deck-header">Deck</div>
			<div class="deck-body" >
				<card
				v-for="(card, index) in cardsDeck"
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
		name: 'Deck',
		components: {
			'card': Card
		},
		props: {
			initRules: Object,
			initCardsDeck: Array,
			initHands: Array
		},
		data() {
			return {
				rules: this.initRules,
				cardsDeck: this.initCardsDeck,
				hands: this.initHands
			}
		},
		methods: {
			generateDeck(){
				this.cardsDeck = []; //reset the deck
				let tempFamily = '';
				for (let i = 0; i < 4; i++){
					switch (i){
						case 0 : tempFamily = "coeur"; break;
						case 1 : tempFamily = "pique"; break;
						case 2 : tempFamily = "trèfle"; break;
						case 3 : tempFamily = "carreau"; break;
					}
					for (let cardValue = 1; cardValue <= 13; cardValue++){
						this.cardsDeck.push({value: cardValue, family: tempFamily});
					}
				}

				//this.$emit('updateDeck', this.cardsDeck)
			},
			shuffleDeck(){
				let tempArr = this.cardsDeck.slice(); //clone the deck
				this.cardsDeck = [];

				for (let i = 0; i < this.rules.cardsNumber; i++){
					let rngIndex = Math.floor(Math.random() * Math.floor(tempArr.length)); //We take a random card from tempArr
					//console.log(rngIndex);
					this.cardsDeck.push(tempArr[rngIndex]); //We add it to our deck
					tempArr.splice(rngIndex, 1);
				}

				this.$forceUpdate;
			},
			distrib(){
				for (let i = 0; i < this.rules.playerNb; i++) {
					this.hands.push([]);
				}
				
				let currentHand = 0;	
				for (let i = 0; i < this.rules.cardsNumber; i++) {
					this.hands[currentHand].push(this.cardsDeck[0]);
					
					this.cardsDeck.shift();

					currentHand++;
					if (currentHand >= this.rules.playerNb){
						currentHand = 0;
					}
				}
			}
		}
	}
</script>

<style>

</style>