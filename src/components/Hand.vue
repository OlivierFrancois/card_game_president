<template>
	<div class="hand">
			<div class="hand-header">Joueur {{ playerIndex + 1 }}</div>
			<div class="hand-body">
				<card
				v-for="(card, index) in cards"
				:key="index"
				:card="card"
				@get-card-by-click="addToSelect(card)"
				></card>

				<div class="cards-selector">
					<card
					v-for="(card, index) in cardsSelect"
					:key="index"
					:card="card"></card>
				</div>
				
				<div class="btnGroup">
					<button class="btn btn-primary mb-1" v-if="playTurn" @click="pushToTrick()">Jouer</button>
					<button class="btn btn-danger" v-if="playTurn" @click="passTurn()">Passer</button>
				</div>
			</div>
		</div>
</template>

<script>
	import Card from './Card'
	import {bus} from '../main'

	export default {
		name: 'Hand',
		components: {
			'card': Card
		},
		data() {
			return{
				cards: this.initCards,
				trick: this.initTrick,
				playersInRound: this.initPlayersInRound,
				cardsSelect: [],
				errorMsg: "",
				roundRule: 0
			}
		},
		props: {
			playerIndex: Number,
			playTurn: Boolean,
			nextTurn: Function,
			initCards : Array,
			initTrick : Array,
			initPlayersInRound : Array
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

				//Player can only play cards with same value
				for(let i = 0; i < this.cardsSelect.length; i++){
					let cardValue = this.cardsSelect[i].value;
					for (let j = 0; j < this.cardsSelect.length; j++){
						if ((j != i) && (this.cardsSelect[j].value != cardValue)){
							this.errorMsg += "ERREUR : Vous ne pouvez pas jouez cette combinaison de cartes."
							return false;
						}
					}
				}

				//If the trick already have cards, the player must play the same number of cards that the first player played
				if (this.roundRule != 0){
					console.log("roundRule actuel : " + this.roundRule);
					if (this.cardsSelect.length != this.roundRule){
						this.errorMsg += "ERREUR : Vous devez respecter le nombre de carte à jouer."
						return false;
					}
				}

				//If the trick already have cards, the player must play something equal or better
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
					console.log(this.errorMsg);
				}
				//Else we push them into the trick
				else {
					//If its the first cards the trick receives, we set the rule of to round with the nb of cards played
					if (this.trick.length <= 0){
						bus.$emit("set-round-rule", this.cardsSelect.length);
					}

					let n = this.cardsSelect.length;
					for(let i = 0; i < n; i++){
						let cardToPush = this.cardsSelect.shift();
						this.trick.push(cardToPush);
					}
					this.nextTurn(); //And we end the turn
				}
			},
			passTurn(){
				//First we remove the index of the player from the playerInRound array
				let index = this.playersInRound.indexOf(this.playerIndex);

				//console.log('Tableau des joueurs dans le round : ', this.playersInRound);
				//console.log('index récupéré : ', index);

				if (index != -1)
					this.playersInRound.splice(index, 1);
				else
					console.log('ERREUR : le joueur n\'est pas dans le tableau');

				//Then we end turn
				this.nextTurn();
			}
		},
		created() {
			bus.$on('get-round-rule', (data) => {
				this.roundRule = data;
			})
		}
	}
</script>

<style>
	.hand-body{
		position: relative;
	}

	.btnGroup{
		position: absolute;
		right: 5%;
		top: 30%;
		width: 5%;
	}

	.btnGroup .btn{
		width: 100%;
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