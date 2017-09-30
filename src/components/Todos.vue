<template>
	<section class="todoapp">
		<header class="header">
			<h1>Todos</h1>
			<input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo"
				   @keyup.enter="addTodo">
		</header>
		<div class="main">
			<input type="checkbox" class="toggle-all" v-model="allDone">
			<ul class="todo-list">
				<li class="todo" v-for="todo in filteredTodos" :class="{completed: todo.completed}">
					<div class="view">
						<input type="checkbox" v-model="todo.completed" class="toggle">
						<label>{{ todo.name }}</label>
						<button class="destroy" @click.prevent="deleteTodo(todo)"></button>
					</div>
				</li>
			</ul>
		</div>
		<footer class="footer">
			<span class="todo-count">
				tâches à faire <strong> {{ remaining }} </strong>
			</span>
			<ul class="filters">
				<li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
				<li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
				<li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
			</ul>
			<button class="clear-completed" v-show="doneTodo" @click.prevent="deleteCompleted">Clear done</button>
		</footer>
	</section>
</template>

<script>
  export default {
    data () {
      return {
        todos: [
          {
            name: 'Premiere tache',
            completed: false
          },
          {
            name: 'Deuxieme tache',
            completed: true
          }
        ],
        newTodo: '',
        filter: 'all'
      }
    },
    methods: {
      addTodo () {
        this.todos.push({
          completed: false,
          name: this.newTodo
        })
        this.newTodo = ''
      },
      deleteTodo (todoDel) {
        this.todos = this.todos.filter(todo => todo !== todoDel)
      }
    },
    computed: {
      remaining () {
        return this.todos.filter(todo => !todo.completed).length
      },
      doneTodo () {
        return this.todos.filter(todo => todo.completed).length
      },
      deleteCompleted () {
        this.todos = this.todos.filter(todo => !todo.completed)
      },
      filteredTodos () {
        if (this.filter === 'todo') {
          return this.todos.filter(todo => !todo.completed)
        } else if (this.filter === 'done') {
          return this.todos.filter(todo => todo.completed)
        } else {
          return this.todos
        }
      },
      allDone: {
        get () {
          return this.remaining === 0
        },
        set (value) {
          this.todos.forEach(todo => {
            todo.completed = value
          })
        }
      }
    }
  }
</script>

<style src="./todo.css"></style>