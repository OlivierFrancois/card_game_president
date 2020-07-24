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

	export default {
		name: 'Trick',
		components: {
			'card': Card
		},
		data() {
			return{
				trickCards: this.initTrick, //Cards in the trick
				roundCardNb: Number //Number of card player have to play in the round
			}
		},
		props: {
			initTrick: Array
		}
	}
</script>

<style>

</style>