<template>
	<div class="model-card my-1 mx-1" @click="$emit('get-card-by-click')">
		<!-- <p>{{ card.value }} <br> {{ card.family }}</p> -->
		<!-- <img src="../assets/2C.png" alt="carte"> -->
		<img :src="require(`@/assets/${card.img}`)" alt="carte">
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
	width: 80px;
	border: black solid 1px;
	background: #f1f1f1;
	align-items: center;
	display: table;
}

.model-card p{
	display: table-cell!important;
	vertical-align: middle!important;
}

.model-card img {
	max-width: 100%;
	max-height: 100;
}

.model-card:hover{
	background: #a09f9f;
}
</style>