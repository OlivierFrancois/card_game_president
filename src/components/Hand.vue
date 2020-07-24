<template>
	<div class="hand">
			<div class="hand-header">Joueur {{ playerIndex + 1 }}</div>
			<div class="hand-body">
				<card
				v-for="(card, index) in cards"
				:key="index"
				:card="card"
				@getCardByClick="addToSelect(card)"
				></card>

				<div class="cards-selector">
					<card
					v-for="(card, index) in cardsSelect"
					:key="index"
					:card="card"></card>
				</div>
				
				<button class="btn btn-primary" v-if="playTurn" @click="pushToTrick()">Jouer</button>
			</div>

			
		</div>
</template>

<script>
	import Card from './Card'

	export default {
		name: 'Hand',
		components: {
			'card': Card
		},
		data() {
			return{
				cards: this.initCards,
				trick: this.initTrick,
				cardsSelect: []
			}
		},
		props: {
			playerIndex: Number,
			playTurn: Boolean,
			initCards : Array,
			initTrick : Array
		},
		methods: {
			addToSelect(card){
				//If it's not the player's turn, we dont do anything
				if (!this.playTurn)
					return;
				let cardIndex = this.cards.indexOf(card);

				this.cardsSelect.push(card);
				this.cards.splice(cardIndex, 1);

				console.log(card);
			},
			pushToTrick(){
				let n = this.cardsSelect.length;
				for(let i = 0; i < n; i++){
					let cardToPush = this.cardsSelect.shift();
					this.trick.push(cardToPush);
				}
			}
		}
	}
</script>

<style>
	.hand-body{
		position: relative;
	}

	.hand-body button{
		position: absolute;
		right: 5%;
		top: 30%;
		width: 5%;
	}

	.cards-selector{
		margin: auto;
		background: #444;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: center;
	}
</style>