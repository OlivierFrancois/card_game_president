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
				roundCardNb: Number //Number of card player have to play in the round
			}
		},
		created(){
			//emited from Hand.vue
			bus.$on('set-round-rule', (data) => {
				this.roundCardNb = data;
				console.log(this.roundCardNb);
				bus.$emit('get-round-rule', this.roundCardNb);
			})
		}
	}
</script>

<style>

</style>