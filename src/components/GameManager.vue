<template>
	<div>
		<button class="btn btn-primary mr-3 my-1" v-on:click="generateDeck()">Générer un deck</button>
		<button class="btn btn-primary mr-3 my-1" v-on:click="shuffleDeck()">Mélanger le deck</button>
		<button class="btn btn-primary mr-3 my-1 " v-on:click="distrib()">Distribuer</button>
		<button class="btn btn-danger mr-3  my-1" v-on:click="reset()">Reset la partie</button>
		
		
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

		<div class="trick">
			<div class="trick-header">Trick</div>
			<div class="trick-body">
				<card
				v-for="(card, index) in trick"
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
			<div class="hand-header">Joueur {{ index + 1 }}</div>
			<div class="hand-body">
				<card
				v-for="(card, index) in hand"
				:key="index"
				:card="card"
				></card>
				
				<button class="btn btn-primary" v-if="playerTurn == index">Jouer</button>
			</div>

			
		</div>
	</div>
</template>

<script>
	import Card from './Card'

	export default {
		name: 'GameManager',
		components: {
			'card': Card,
		},
		data() {
			return{
				cardsNumber: 52,
				cardsDeck: [],
				playerNb: 4,
				hands: [],
				trick: [],
				playerTurn : 0,
				playerSelection: []
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
			},
			reset(){
				this.cardsDeck = [];
				this.hands = [];
				this.trick = [];
				this.playerSelection = [];
				this.playerTurn = 0;
			},
			shuffleDeck(){
				let tempArr = this.cardsDeck.slice(); //clone the deck
				this.cardsDeck = [];

				for (let i = 0; i < this.cardsNumber; i++){
					let rngIndex = Math.floor(Math.random() * Math.floor(tempArr.length)); //We take a random card from tempArr
					//console.log(rngIndex);
					this.cardsDeck.push(tempArr[rngIndex]); //We add it to our deck
					tempArr.splice(rngIndex, 1);
				}

				this.$forceUpdate;
			},
			distrib(){
				for (let i = 0; i < this.playerNb; i++) {
					this.hands.push([]);
				}
				
				let currentHand = 0;	
				for (let i = 0; i < this.cardsNumber; i++) {
					this.hands[currentHand].push(this.cardsDeck[0]);
					
					this.cardsDeck.shift();

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
	.deck, .hand, .trick{
		width: 90%;
		margin: auto;
		margin-top: 10px;
	}
	
	.deck-body, .hand-body, .trick-body{
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		background: #333;
		padding: 1%;
	}

	.hand-header, .deck-header, .trick-header{
		background: #222;
		color:lightseagreen;
	}
	
	/* TRICK */
	.trick-body {
		min-height: 50px;
	}

	/* HAND */
	.hand-body{
		position: relative;
	}

	.hand-body button{
		position: absolute;
		right: 5%;
		top: 30%;
		width: 5%;
	}
</style>