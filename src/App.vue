<template lang="pug">
  #app
    .bg-primary
      .container.justify-content-between.d-flex.todo-nav
        a.active(href="#") My Task
        a(href="#" ) In Progress
        a(href="#") Completed
    .container.my-4
      .position-relative(v-if="isNewTodo === false")
        i.fas.fa-plus.fa-lg.text-black-50.position-absolute(style="left: 1rem; top: 1.15rem")
        input.form-control.form-control-lg.pl-5(type="text" @click="isNewTodo = true")
      .mt-4
        //- @closeEditTodo="closeEdit"當內層closeEditTodo被觸發時會觸發外層closeEdit事件
        edit-todo-item(v-if="isNewTodo === true" @closeEditTodo="closeEdit")
        //- draggable要包在互動元件的最外層
        //- @end 結束時觸發的事件 v-model則綁定變動的陣列 :options限制可以拖曳的區塊
        draggable(@end="dragItem" :options="{handle: '.handle'}" v-model="todos")
          todo-item(:todo="item" v-for="(item, key) in todos" :key="key" @addStared="getData")

</template>

<script>
import TodoItem from './components/TodoItem.vue'
import EditTodoItem from './components/EditTodoItem.vue'
import draggable from 'vuedraggable'

export default {
	name: 'App',
	data () {
		return {
			todos: [
				{
					message: '第一筆資訊',
					startDate: '2018-06-06',
					endDate: '2018-06-28',
					timestamp: 1528254270,
					stared: false,
					comments: [ ],
					id: 2,
					completed: 'progress'
				}
			],
			isNewTodo: false,
			sortData: []
		}
	},
	components: {
		TodoItem,
		EditTodoItem,
		draggable
	},
	created () {
		this.getData()
	},
	methods: {
		closeEdit () {
			this.isNewTodo = false
			this.getData()
		},
		getData () {
			const self = this
			const api = 'http://localhost:3000/todos'
			self.$http.get(api).then((res) => {
				console.log(res)
				self.todos = res.data
				return self.$http.get('http://localhost:3000/sort')
					.then((res) => {
						// console.log(res)
					})
			})
		},
		dragItem () {
			console.log(this.todos)
			const self = this
			const api = 'http://localhost:3000/sort'
			const sort = self.todos.map((item) => {
				// console.log(item)
				// 回傳排序後的id
				return item.id
			})
			// console.log(sort)
			self.$http.post(api, {sort: sort}).then((res) => {
				console.log(res)
				// self.todos = res.data
			})
		}
	}
}
</script>

<style lang="sass">
	.draggable .handle
		cursor: move
</style>
