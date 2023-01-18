<script setup>
	import {
		ref
	} from 'vue'

	const prop = defineProps({
		msg: String,
		hello:String
	})
	
	const emit = defineEmits(['update:hello'])
	
	const changeText = () =>{
		emit('update:hello','hello 123')
	}
</script>

<template>
	<h1>{{ msg }}--{{hello}}</h1>
	<button @click="changeText">改变内容</button>

</template>

<style scoped>
	a {
		color: #42b983;
	}
</style>
