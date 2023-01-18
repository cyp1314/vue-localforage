<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import { ref, watch, onMounted } from 'vue';
import localforage from 'localforage';
import MyCus from './components/MyCus.vue';
const msg = ref('hello world');
const imageUrl = ref('');
const hello = ref('test1');
watch(hello, () => {
	console.log('change...');
});
const test = () => {
	localforage.getItem('k6', function(err, value) {
		const blob = value;
		imageUrl.value = (window.URL || window.webkitURL).createObjectURL(blob);
	});
};
const filechange = () => {
	let files = document.getElementById('shangchuan').files[0];
	let type = files.type;
	console.log('files', files);
	geturl(files, type);
};

const geturl = (files, type) => {
	let reads = new FileReader();
	reads.readAsArrayBuffer(files);
	reads.onload = function(e) {
		localforage
			.setItem('k6', new Blob([reads.result], { type: type }))
			.then(function(value) {
				// 当值被存储后，可执行其他操作
				console.log(value);
			})
			.catch(function(err) {
				// 当出错时，此处代码运行
				console.log(err);
			});
		console.log(reads.result);
		console.log(new Blob([reads.result], { type: type }));
		// 注意：里面的this指的就是reads，所以reads.result == this.result
		//document.getElementById("img").src = this.result;
	};
};
onMounted(() => {
	// This is what our customer data looks like.
	// const customerData = [
	//   { ssn: "444-44-4444", name: "Bill", age: 35, email: "bill@company.com" },
	//   { ssn: "555-55-5555", name: "Donna", age: 32, email: "donna@home.org" }
	// ];

	// localforage.setItem('k2', customerData).then(function (value) {
	//     // 当值被存储后，可执行其他操作
	//     console.log(value);
	// }).catch(function(err) {
	//     // 当出错时，此处代码运行
	//     console.log(err);
	// });

	// 回调版本：
	localforage.getItem('k2', function(err, value) {
		// 当离线仓库中的值被载入时，此处代码运行
		let obj = value;
		console.log(obj);
	});
});
</script>

<template>
	<img alt="Vue logo" src="./assets/logo.png" />
	<MyCus v-model:hello="hello" :msg="msg"></MyCus>
	<div style="border: 1px solid red;height: 20px;width: 200px;"><p style="font-size: 16px;line-height: 0;padding: 0 12px;">hello world</p></div>
	<input type="file" id="shangchuan" @change="filechange" />
	<div class="tup"><img id="img" src="" /></div>
	<button @click="test">test</button>
	<img  :src="imageUrl"/>
</template>

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
