<template>
	<div>
		<deck
		:initRules="rules"
		:initCardsDeck="cardsDeck"
		:initHands="hands"></deck>

		<button class="btn btn-danger mr-3  my-1" v-on:click="reset()">Reset la partie</button>

		<trick :initTrick="trick"></trick>

		<br>
		
		<hand v-for="(hand, index) in hands" :key="index"
		:initCards="hands[index]"
		:initTrick="trick"
		:nextTurn="nextTurn"
		:playTurn="playerTurn == index"
		:initPlayersInRound="playersInRound"
		:playerIndex="index"></hand>
	</div>
</template>

<script>
	import Deck from './Deck'
	import Hand from './Hand'
	import Trick from './Trick'
	import {bus} from '../main'

	export default {
		name: 'GameManager',
		components: {
			'deck': Deck,
			'trick': Trick,
			'hand': Hand
		},
		data() {
			return{
				rules: {cardsNumber: 52, playerNb: 4},
				cardsDeck: [],
				hands: [],
				trick: [],
				playerTurn: 0,
				playerSelection: [],
				playersInRound: []
			}
		},
		methods: {
			updateDeck(updatedDeck){
				this.cardsDeck = updatedDeck;
			},
			resetPlayersInRound(){
				this.playersInRound.splice(0, this.playersInRound.length);

				for (let i = 0; i < this.rules.playerNb; i++) {
					this.playersInRound.push(i);
				}
			},
			reset(){
				this.cardsDeck.splice(0, this.cardsDeck.length);
				this.hands.splice(0, this.hands.length);
				this.trick.splice(0, this.trick.length);
				this.playerSelection.splice(0, this.playerSelection.length);
				this.playerTurn = 0;
				this.playersInRound.splice(0, this.playersInRound.length);

				this.resetPlayersInRound();
			},
			nextTurn(){
				//console.log(this.playersInRound);
				//We reset a variable inside every hand through this event
				bus.$emit("set-x-or-pass", false);

				//If there is only 1 player left in the round, he wins the round and we start the next one
				if (this.playersInRound.length <= 1){
					this.nextRound();
				}
				else {
					//If the last 4 cards played have the same value, the round just ends
					if (this.trick.length >= 4) {
						let tempLength = this.trick.length;
						let exitLoop = true;
						let i = 1;
						do {
							exitLoop = this.trick[tempLength - 1].value == this.trick[tempLength - i].value;
							i++;
						} while (exitLoop && (i <= 4));

						if (exitLoop) {
							this.nextRound();
							return;
						}
					}

					let playerHasPassed = true; //Used to leave the loop
					let i = 0; //Used to leave the loop if every players have passed

					//We repeat that until we get a player that has not passed or until we checked every players
					while ((playerHasPassed) && (i < this.playersInRound.length)) {
						//First we get the next player
						(this.playerTurn >= this.rules.playerNb - 1) ? (this.playerTurn = 0) : (this.playerTurn++);

						//Then we check if this player is in the round
						let j = 0;
						while ((playerHasPassed) && (j < this.playersInRound.length)) {
							playerHasPassed = (this.playerTurn != this.playersInRound[j]);
							j++;
						}
						i++;
					}
				}
			},
			nextRound(){
				this.resetPlayersInRound(); //Reset players in round
				bus.$emit("set-x-or-pass", false);
				this.trick.splice(0, this.trick.length); //Clear the trick
				bus.$emit("set-round-rule", 0);
				console.log('NEXT ROUND');
			}
		},
		created(){
			this.resetPlayersInRound();
		},
		updated(){
			if (this.playersInRound.length <= 0){
				console.log('Il faut reset le round');
			}
		}
	}
</script>

<style>
	.deck, .trick{
		width: 90%;
		margin: auto;
		margin-top: 10px;
	}
	
	.deck-body, .trick-body{
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		background: #333;
		padding: 1%;
		justify-content: center;
	}

	.deck-header, .trick-header{
		background: #222;
		color:lightseagreen;
	}
	
	/* TRICK */
	.trick-body {
		min-height: 50px;
	}
</style>