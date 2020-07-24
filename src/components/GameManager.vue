<template>
	<div>
		<deck
		:initRules="rules"
		:initCardsDeck="cardsDeck"
		:initHands="hands"></deck>

		<trick :initTrick="trick"></trick>

		<br>
		
		<hand v-for="(hand, index) in hands" :key="index"
		:initCards="hands[index]"
		:initTrick="trick"
		:playTurn="playerTurn == index"
		:playerIndex="index"></hand>
	</div>
</template>

<script>
	import Deck from './Deck'
	import Hand from './Hand'
	import Trick from './Trick'

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
				playerTurn : 0,
				playerSelection: []
			}
		},
		methods: {
			updateDeck(updatedDeck){
				this.cardsDeck = updatedDeck;
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
		justify-content: center;
	}

	.hand-header, .deck-header, .trick-header{
		background: #222;
		color:lightseagreen;
	}
	
	/* TRICK */
	.trick-body {
		min-height: 50px;
	}
</style>