<template>
	<div id="app">
		<header-top></header-top>
		<game-manager></game-manager>
		<br><br>
	</div>
</template>

<script>
	import Header from './components/Header'
	import GameManager from './components/GameManager'

	export default {
		name: 'App',
		components: {
			'header-top': Header,
			'game-manager': GameManager
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