<template>
	<div class="hand">
			<div class="hand-header">Joueur {{ playerIndex + 1 }}</div>
			<div class="hand-body">
				<card
				v-for="(card, index) in cards"
				:key="index"
				:card="card"
				></card>
				
				<!--
				<button class="btn btn-primary" v-if="playerTurn == index">Jouer</button>
				-->
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

			}
		},
		props: {
			playerIndex: Number,
			cards : Array
		}
	}
</script>

<style>

</style>