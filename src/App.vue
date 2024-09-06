<template>
	<h1 id="first_title">Список задач</h1>
	<TasksBlock />
</template>

<script>
import TasksBlock from './components/TasksBlock.vue';

export default {
	name: 'App',
	components: {
		TasksBlock,
	},
};
</script>

<style>
#first_title {
	text-align: center;
	margin-top: 70px;
}
</style>
