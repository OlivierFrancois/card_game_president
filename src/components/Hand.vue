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
				cardsSelect: [],
				errorMsg: ""
			}
		},
		props: {
			playerIndex: Number,
			playTurn: Boolean,
			nextTurn: Function,
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

				//console.log(card);
			},
			validateSelect(){
				this.errorMsg = ''; //reset the errorMsg

				//Player can only play card with same value
				for(let i = 0; i < this.cardsSelect.length; i++){
					let cardValue = this.cardsSelect[i].value;
					for (let j = 0; j < this.cardsSelect.length; j++){
						if ((j != i) && (this.cardsSelect[j].value != cardValue)){
							this.errorMsg += "ERREUR : Vous ne pouvez pas jouez cette combinaison de cartes."
							return false;
						}
					}
				}

				//If the trick already has cards, the player must play something equal or better
				if (this.trick.length > 0){
					let selectedCardValue = this.cardsSelect[0].value;
					let trickCardValue = this.trick[this.trick.length -1].value;
					if (selectedCardValue < trickCardValue){
						this.errorMsg += "ERREUR : Carte(s) trop faible(s)"
						return false;
					}
				}
				
				return true;
			},
			pushToTrick(){
				//If the selection isnt valid, we send back the cards selected to the hand
				if (!this.validateSelect()){
					let n = this.cardsSelect.length;
					for(let i = 0; i < n; i++){
						let cardToPush = this.cardsSelect.shift();
						this.cards.push(cardToPush);
					}
				}
				//Else we push them into the trick
				else {
					let n = this.cardsSelect.length;
					for(let i = 0; i < n; i++){
						let cardToPush = this.cardsSelect.shift();
						this.trick.push(cardToPush);
					}
					this.nextTurn(); //And we end the turn
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