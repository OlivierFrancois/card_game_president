<template>
	<div class="trick">
			<div class="trick-header">Trick</div>
			<div class="trick-body">
				<card
				v-for="(card, index) in trickCards"
				:key="index"
				:card="card"
				></card>
			</div>
		</div>
</template>

<script>
	import Card from './Card'
	import {bus} from '../main'

	export default {
		name: 'Trick',
		components: {
			'card': Card
		},
		props: {
			initTrick: Array
		},
		data() {
			return{
				trickCards: this.initTrick, //Cards in the trick
				roundRule: Number, //Number of card player have to play in the round
				playersLeft: Number,
				xOrPassTrick: false
				/*skipNextPlayer: Boolean*/
			}
		},
		created(){
			//emited from Hand.vue
			bus.$on('set-round-rule', (data) => {
				this.roundRule = data;
				// console.log("data : " + data);
				// console.log("roundRule editée : " + this.roundRule);
				bus.$emit('get-round-rule', this.roundRule);
			})

			bus.$on('decrease-player-left', () => {
				this.playersLeft--;
			})
		},
		updated(){
			if ((this.roundRule == 1) && (this.trickCards.length >= 2) && (!this.xOrPassTrick)) {
				let length = this.trickCards.length;
				if (this.trickCards[length - 1].value == this.trickCards[length - 2].value){
					this.xOrPassTrick = true;
					bus.$emit('set-x-or-pass', 'true');
				}
			}

			bus.$on('set-x-or-pass-trick', (data) => {
				this.xOrPassTrick = data;
			})
		}
	}
</script>

<style>

</style>