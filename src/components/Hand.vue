<template>
	<div class="hand" >
			<div class="hand-header" @dblclick="sortHand()" >Joueur {{ playerIndex + 1 }}</div>
			<div class="hand-body" :class="{ hasPassed : !playTurn }">
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
					<button class="btn btn-warning" v-if="playTurn && xOrPass" @click="nextTurn(false, false)">Passer</button>
					<button class="btn btn-danger" v-if="playTurn && (trick.length != 0)" @click="nextTurn(true, false)">Se coucher</button>
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
				cardsSelect: [], //cards player want to send to the trick
				errorMsg: "",
				roundRule: 0, //number of cards player have to play
				xOrPass: false //To handle the case where the previous 2 cards have the same value
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
			sortHand(){
				this.cards.sort((a, b) => {
					if ((a.value) > (b.value))
						return 1;
					else
						return -1;
				})
			},
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
					//console.log("roundRule actuel : " + this.roundRule);
					if (this.cardsSelect.length != this.roundRule){
						this.errorMsg += "ERREUR : Vous devez respecter le nombre de carte à jouer."
						return false;
					}
				}

				//If the last 2 cards are the same value, the player has to play those value
				if (this.xOrPass) {
					let length = this.trick.length;
					if (this.cardsSelect[this.cardsSelect.length - 1].value != this.trick[length - 1].value) {
						this.errorMsg += "ERREUR : " + this.trick[length - 1].value + " ou passe !";
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

					//Before we push the cards, we check if the previous played cards are the same. If yes, we reset set-x-or-pass-trick
					let tempLength = this.trick.length;
					if ((tempLength >= 2) && (this.trick[tempLength - 1].value == this.trick[tempLength - 2].value)){
						bus.$emit("set-x-or-pass-trick", false);
					}

					//Now we push the selected cards into the trick
					let n = this.cardsSelect.length;
					for(let i = 0; i < n; i++){
						let cardToPush = this.cardsSelect.shift();
						this.trick.push(cardToPush);
					}

					this.nextTurn(false, true); //And we end the turn
				}
			}
		},
		created() {
			bus.$on('get-round-rule', (data) => {
				this.roundRule = data;
			})
		},
		updated() {
			bus.$on('set-x-or-pass', (data) => {
				this.xOrPass = data;
			})
		}
	}
</script>

<style>
	.hand{
		width: 90%;
		margin: auto;
		margin-top: 10px;
	}

	.hand-header{
		background: #222;
		color:lightseagreen;
	}

	.hand-body{
		position: relative;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		background: #333;
		padding: 1%;
		justify-content: center;
		align-items: center;
	}

	.btnGroup{
		position: relative;
		top: 30%;
		width: 30%;
		display: flex;
		margin-left: 50px;
	}

	.btnGroup .btn{
		width: 150px;
		margin: 5px;
		height: 40px;
	}

	.cards-selector{
		margin: auto;
		background: #444;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: center;
	}

	.hasPassed{
		background: #292929!important;
	}
</style>