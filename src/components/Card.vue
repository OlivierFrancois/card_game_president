<template>
	<div class="model-card my-1 mx-auto">
		<p>{{ card.value }} de {{ card.family }}</p>
	</div>
</template>

<script>
	export default {
		name: 'Card',
		data() {
			return{
				
			}
		},
		props: {
			card : {type: Object}
		}
	}
</script>

<style scoped>
.model-card{
	height: 200px;
	width: 150px;
	border: black solid 1px;
}
</style>