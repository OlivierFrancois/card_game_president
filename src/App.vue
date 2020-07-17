<template>
	<div id="app">
		<header-top></header-top>
		<cards-deck></cards-deck>
		<hand></hand>
	</div>
</template>

<script>
	import Header from './components/Header'
	import CardsDeck from './components/CardsDeck'
	import Hand from './components/Hand'

	export default {
		name: 'App',
		components: {
			'header-top': Header,
			'cards-deck': CardsDeck,
			'hand': Hand,
		}
	}
</script>

<style>
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}
</style>