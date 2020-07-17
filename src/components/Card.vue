<template>
	<div class="card model-card my-1 mx-auto">
		<p>{{ value }} de {{ family }}</p>
	</div>
</template>

<script>
	export default {
		name: 'Card',
		data() {
			return{
				
			}
		},
		props: ['value', 'family']
	}
</script>

<style scoped>
.model-card{
	height: 150;
	width: 500px;
}
</style>