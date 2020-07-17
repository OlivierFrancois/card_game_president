<template>
	<div class="model-card my-1 mx-1">
		<p>{{ card.value }} <br> {{ card.family }}</p>
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
	height: 120px;
	width: 60px;
	border: black solid 1px;
	background: #f1f1f1;
	align-items: center;
	display: table;
}

.model-card p{
	display: table-cell!important;
	vertical-align: middle!important;
}
</style>