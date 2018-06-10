<template lang="pug">
.mb-4
	.todo-item(:class="{stared: todo.stared}")
		.handle.text-muted
			i.fas.fa-ellipsis-v
		.todo-header
			.todo-check
				input(type="checkbox" :id="todo.id")
				label(:for="todo.id")
			label(class="todo-title")
				span {{ todo.message }}
			.todo-control
				a(href="#" class="text-muted" @click.prevent="updateStar")
					i.far.fa-star.mr-3(v-if="!todo.stared")
					i.fas.fa-star.mr-3(v-else)
				a(href="#" class="text-muted")
					i.fas.fa-pencil-alt
		.todo-footer.text-secondary
			i.far.fa-calendar-alt &nbsp
			span form: 2018-06-28
			span ~
			span to: 2018-06-28
			//- i.far.fa-file.ml-3
			i.fas.fa-comment-dots.ml-3 &nbsp3
</template>

<script>
export default{
	props: ['todo'],
	name: 'Todo',
	methods: {
		updateStar () {
			const self = this
			const api = `http://localhost:3000/todos/${self.todo.id}`
			const todo = {
				...self.todo
			}
			todo.stared = !self.todo.stared
			self.$http.put(api, todo).then((res) => {
				console.log(res)
				// 把星星新增上去後 呼叫外層的getData刷新資料
				self.addStarStyle()
			})
		},
		addStarStyle () {
			this.$emit('addStared')
		}
	}
}
</script>
