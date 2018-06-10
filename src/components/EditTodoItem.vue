<template lang="pug">
.mb-4
	.todo-item.active
		.todo-header
			//- .todo-check
			//-   input(type="checkbox" id="123")
			//-   label(for="123")
			label.todo-title
				input(type="text" value="可輸入內容" class="form-control" v-model="cacheTodo.message")
		.todo-body
			.todo-content
				.todo-row
					.todo-icon
						i.far.fa-calendar-alt
					div
						label(for="") Deadline
						.form-inline.todo-form-control
							input.form-control.border-0(type="date")
							input.form-control.border-0.ml-2(type="date")
				.todo-row
					.todo-icon
						i.far.fa-comment-dots
					.todo-form-control
						label(for="") Comment
						.bg-white.p-3.my-3 留言訊息
						div
							textarea.form-control.w-100.border-0(name="")
		.todo-btn-group.d-flex
			button.btn.text-danger.w-50.btn-lg
				i.fas.fa-times &nbsp
				|Cancel
			button.btn.btn-primary.w-50.btn-lg(@click="updateTodo")
				i.fas.fa-plus &nbsp
				|Update Task
</template>

<script>
export default{
	// props拿來接收外層資料
	props: ['todo'],
	name: 'EditTodo',
	data () {
		return {
			cacheTodo: {}
		}
	},
	methods: {
		updateTodo () {
			console.log(this.cacheTodo)
			const self = this
			const api = 'http://localhost:3000/todos'
			const todo = {
				...self.cacheTodo,
				stared: false,
				compeleted: 'progress'
			}
			self.$http.post(api, todo).then((res) => {
				console.log(res)
				self.closeTodo()
			})
		},
		closeTodo () {
			// $emit用來觸發外層事件
			this.$emit('closeEditTodo')
		}
	}
}
</script>
