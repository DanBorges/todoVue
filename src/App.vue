<template>
	<div id="app">
		<h1>Tarefas</h1>
		<TaskProgress :progress="progress" />
		<NewTask @taskAdd="newTask"  />
	<TaskGrid :tasks="tasks" @removeTask="removeTask" @changeStatus="changeStatus"/>
	</div>
</template>

<script>
import TaskGrid from './components/TaskGrid.vue'
import NewTask from './components/NewTask'
import TaskProgress from './components/TaskProgress'
export default {
	components: {
		TaskGrid,
		NewTask,
		TaskProgress
	},
	data() {
		return {
			tasks: []
		}
	},

	created() {
		const json = localStorage.getItem('tasks')
		const array = JSON.parse(json)
		this.tasks = Array.isArray(array) ? array : []

	},

	computed: {
		progress() {
			const total= this.tasks.length
			const done = this.tasks.filter(t=>t.pending === true).length
			return Math.round(done/total *100) || 0
		}
	},

	methods: {
		newTask(task) {
			const sameName = t => t.name === task.name
			const reallyName = this.tasks.filter(sameName).length ==0
			reallyName && this.tasks.push({
				name: task.name,
				pending: task.pending || false
			})
		},

		removeTask(i) {
			this.tasks.splice(i, 1)
		},
		changeStatus (i) {
			this.tasks[i].pending = !this.tasks[i].pending
		}
	},

	watch: {
		tasks: {
			depp: true,
			handler() {
				localStorage.setItem('tasks', JSON.stringify(this.tasks))
			}
		}
	}

}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}

	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>
