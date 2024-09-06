<template>
	<div id="newTasksBlock">
		<input
			@keyup.enter="add_new()"
			type="text"
			name=""
			id=""
			v-model="nametask"
			placeholder="Введите новую задачу"
			maxlength="23"
		/>
		<br />
		<p id="p_name_task">Задача будет называться:<br />{{ nametask }}</p>
		<button @click="add_new()">Добавить</button>
	</div>
	<div id="center_filter_ask">
	<h1>Фильтрация задач по:</h1><br/>
	<div class="flex-row">
	<input type="radio" @change="filter_activate()" checked name="type_tasks" id="radio_all">
	<p>Все</p>
</div>
<div class="flex-row">
	<input type="radio" @change="filter_activate()" name="type_tasks" id="radio_active">
	<p>Активные</p>
</div>
<div class="flex-row">
	<input type="radio" @change="filter_activate()" name="type_tasks" id="radio_accomp">
	<p>Выполненные</p>
</div>
</div>
	<div id="block_ready_posts">
		<div v-for="(item, index) in items" :key="item.id" :class="'block_posts ' + item.status" :id="'block_posts_' + index" @dblclick="edit_task(index)">
			<p :id="'p_block_posts_' + index" v-if="item.status === 'active'">{{ item.title }}</p>
			<p :id="'p_block_posts_' + index" class="text_closed" v-if="item.status !== 'active'">{{ item.title }}</p>
			<input type="text" name="" :id="'input_edit_block_posts_' + index" v-model="item.title"><br/>
			<button :id="'edit_block_posts_' + index" @click="confirm_edit(index)" class="button_green edit_button">Изменить</button>
			<br />
			<button v-if="item.status !== 'active'" @click="returns(index)" class="button_green">Восстановить</button>
			<button v-if="item.status === 'active'" @click="close(index)" class="button_green">Завершить</button><br />
			<button @click="delete_block(index)" class="button_red">Удалить</button>
		</div>
	</div>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const items = ref([]);

onMounted(() => {
	if(localStorage.getItem('items')){
	items.value = JSON.parse(localStorage.getItem('items'));
	}
});

function add_new() {
	items.value.push({ title: nametask.value, status: 'active' });
	nametask.value = '';
	localStorage.setItem('items', JSON.stringify(items.value));
}

function delete_block(number){
	items.value.splice(number, 1);
	localStorage.setItem('items', JSON.stringify(items.value));
}

function confirm_edit(element){
	document.getElementById("p_block_posts_" + element).style.display = "inline";
	document.getElementById("input_edit_block_posts_" + element).style.display = "none";
	document.getElementById("edit_block_posts_" + element).style.display = "none";
	localStorage.setItem('items', JSON.stringify(items.value));
}

function edit_task(element){
	document.getElementById("p_block_posts_" + element).style.display = "none";
	document.getElementById("input_edit_block_posts_" + element).style.display = "inline";
	document.getElementById("edit_block_posts_" + element).style.display = "inline";
}

function close(number){
	var status_button;
	var value_title = items.value[number].title;
	console.log(value_title);
	items.value.splice(number, 1);
	items.value.push({title: value_title, status: 'none'})
	console.log(status_button);
	localStorage.setItem('items', JSON.stringify(items.value));
}

function filter_activate(){
	if(document.getElementById("radio_all").checked){
		var length_active = document.getElementsByClassName("active");
		for(let i = 0; i < length_active.length; i++){
			document.getElementsByClassName("active")[i].style.display = "block";
		}
		length_active = document.getElementsByClassName("none"); // for none
		for(let i = 0; i < length_active.length; i++){
			document.getElementsByClassName("none")[i].style.display = "block";
		}
	}
	if(document.getElementById("radio_active").checked){
		var length_none = document.getElementsByClassName("active");
		for(let i = 0; i < length_none.length; i++){
			document.getElementsByClassName("active")[i].style.display = "block";
		}
		length_none = document.getElementsByClassName("none"); // for none
		for(let i = 0; i < length_none.length; i++){
			document.getElementsByClassName("none")[i].style.display = "none";
		}
	}
	if(document.getElementById("radio_accomp").checked){
		var length_accomp = document.getElementsByClassName("active");
		for(let i = 0; i < length_accomp.length; i++){
			document.getElementsByClassName("active")[i].style.display = "none";
		}
		length_accomp = document.getElementsByClassName("none"); // for none
		for(let i = 0; i < length_accomp.length; i++){
			document.getElementsByClassName("none")[i].style.display = "block";
		}
	}
}

function returns(number){
	var status_button;
	var value_title = items.value[number].title;
	console.log(value_title);
	items.value.splice(number, 1);
	items.value.push({title: value_title, status: 'active'})
	console.log(status_button);
	localStorage.setItem('items', JSON.stringify(items.value));
}
const nametask = ref('');
</script>

<style scoped>
button:hover {
	cursor: pointer;
	text-decoration: underline;
}
#block_ready_posts{
	margin-bottom: 50px;
}
.edit_button{
	display: none;
}
#block_ready_posts input{
	display: none;
	font-size: 20px;
	padding: 10px;
	border-radius: 10px;
	border-color: rgb(204, 204, 204);
}
#newTasksBlock {
	padding: 20px;
	text-align: center;
	display: block;
	margin-left: auto;
	margin-right: auto;
	border: 2px solid rgb(204, 204, 204);
	max-width: max-content;
	border-radius: 20px;
	margin-top: 30px;
}
#newTasksBlock input {
	font-size: 20px;
	padding: 10px;
	border-radius: 20px;
	border: 2px solid rgb(204, 204, 204);
}
#newTasksBlock button {
	padding: 10px;
	border-radius: 20px;
	background-color: green;
	color: white;
	font-size: 20px;
	margin-top: 20px;
	padding-left: 40px;
	padding-right: 40px;
	border: none;
}
#newTasksBlock button:hover {
	cursor: pointer;
	text-decoration: underline;
}
#p_name_task {
	margin-top: 20px;
	font-size: 20px;
}
.block_posts {
	text-align: center;
	display: block;
	margin-left: auto;
	margin-right: auto;
	font-size: 30px;
	border-radius: 20px;
	margin-top: 20px;
	flex-direction: column;
	padding: 20px;
	border: 2px solid rgb(204, 204, 204);
	max-width: 400px;
}
.block_posts:hover{
	cursor: pointer;
	border-color: rgb(134, 134, 134);
}
.block_posts .button_red {
	background-color: rgb(192, 31, 31);
	color: white;
	font-size: 20px;
	padding: 10px;
	border-radius: 20px;
	border: none;
	margin-bottom: 30px;
}
.block_posts .button_green {
	background-color: green;
	color: white;
	font-size: 20px;
	padding: 10px;
	border-radius: 20px;
	border: none;
	margin-bottom: 15px;
	margin-top: 20px;
}
.text_closed{
	text-decoration: line-through;
}
#center_filter_ask{
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
}
.flex-row{
	display: flex;
	flex-direction: row;
	align-items: start;
	justify-content: left;
	width: 300px;
	margin-bottom: 20px;
}
input[type="radio"]{
	width: 24px;
	height: 24px;
}
.flex-row p{
	font-size: 20px;
	margin-left: 12px;
}
</style>
