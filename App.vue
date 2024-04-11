<script>
export default {
  data (){
    return {
      todos: [
        {
          id: 1,
          title: '整理离散相数据',
          compeleted: false,

        },
        {
          id: 2,
          title: '制作论文时序图',
          compeleted: true,
        }
      ],
      visibility: 'all',
    }
  },
  methods:{
    toggleAll(e){
      this.todos.forEach(todo => {
        todo.compeleted = e.target.checked
      })
      // 小箭头点选全部checked状态
    },
    onHashChange(){
      const hash = window.location.hash.replace(/#\/?/, '')
      if (['all', 'active', 'completed'].includes(hash)) {
        this.visibility = hash
    } else {
      window.location.hash = ''
      this.visibility = 'all'
    }
    },
    addTodo(e){
      const title = e.target.value.trim()
      if(!title){
        return 
      }
      this.todos.push({
        id: Date.now(),
        title,
        compeleted: false
      })
      e.target.value = ''
    },
    removeTodo(e){
      this.todos.splice(this.todos.indexOf(e), 1)
    },
    clearCompleted(){
      this.todos = this.todos.filter(todo => !todo.compeleted)
    },
    updateTitle(e){
      const index = this.todos.indexOf(e)
      if (index != -1){
        this.todos[index].title = e.target.value
      }
    }
  
  },
computed: {
  filteredTodos(){
    switch(this.visibility) {
      case 'all':
        return this.todos
      case 'active':
        return this.todos.filter(todo => !todo.compeleted)
      case 'completed':
        return this.todos.filter(todo => todo.compeleted)
    }
  },
  remaining (){
    let unit = this.todos.filter(todo => !todo.compeleted).length > 1 ? 'items' : 'item'
    let strArr = [this.todos.filter(todo => !todo.compeleted).length, unit]
    return strArr.join(" ")
  },
  showfooter (){
    if (this.todos.length != 0){
      return true
    } else{
      return false
    }
  }
},
mounted (){
    // hash处理
    window.addEventListener('hashchange', this.onHashChange)
    this.onHashChange()
  }
}
</script>

<template>
  <section class="todoapp">
			<header class="header">
				<h1>KWKTodos</h1>
				<input @keyup.enter="addTodo" class="new-todo" placeholder="What needs to be done?" autofocus>
			</header>
			<!-- This section should be hidden by default and shown when there are todos -->
			<section class="main">
				<input id="toggle-all" class="toggle-all" type="checkbox" @click="toggleAll">
				<label for="toggle-all">Mark all as complete</label>
				<ul class="todo-list">
					<!-- These are here just to show the structure of the list items -->
					<!-- List items should get the class `editing` when editing and `completed` when marked as completed -->
					<li 
            v-for="todo in filteredTodos"
            :key="todo.id"
            :class="{ completed: todo.compeleted }"
            >
						<div class="view">
							<input class="toggle" type="checkbox" v-model="todo.compeleted">
             
							<label v-text="todo.title"
                     @input="todo.title = $event.target.innerText"
                     contenteditable ></label>
							<button class="destroy" @click="removeTodo"></button>
						</div>
						<input class="edit" value="Create a TodoMVC template">
					</li>
				</ul>
			</section>
			<!-- This footer should be hidden by default and shown when there are todos -->
			<footer class="footer" v-show="showfooter">
				<!-- This should be `0 items left` by default -->
				<span class="todo-count"><strong>{{ remaining }}</strong> left</span>
				<!-- Remove this if you don't implement routing -->
				<ul class="filters">
					<li>
						<a :class="{ selected: visibility === 'all' }" href="#/all">All</a>
					</li>
					<li>
						<a :class="{ selected: visibility === 'active' }" href="#/active">Active</a>
					</li>
					<li>
						<a :class="{ selected: visibility === 'completed' }" href="#/completed">Completed</a>
					</li>
				</ul>
				<!-- Hidden if no completed items are left ↓ -->
				<button class="clear-completed" @click="clearCompleted">Clear completed</button>
			</footer>
		</section>
</template>

<style scoped>

@import "https://unpkg.com/todomvc-app-css@2.4.1/index.css";

</style>