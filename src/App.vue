<template>
  <div id="app">
    <h1>todo list1</h1>
    <h2 id="list-summary">{{ listSummary }}</h2>
    <TodoForm v-on:todo-added="addTodo"></TodoForm>
    <ul aria-labelledby="list-summary" class="stack-large">
      <li v-for="item in todoItmes" v-bind:key="item.id">
        <TodoItem v-bind:label="item.label" v-bind:done="item.done" :id="id"
                  @checkbox-changed="updateDoneStatus(item.id)"
                  @item-edited="todoEdit(item.id, $event)" @item-deleted="todoDeleted(item.id)"/>
      </li>
    </ul>
  </div>
</template>

<script>
import uniqueId from 'lodash.uniqueid'
import TodoItem from './components/TodoItem.vue'
import TodoForm from './components/TodoForm.vue'

export default {
  name: 'App',
  components: {TodoItem, TodoForm},
  data () {
    return {
      todoItmes: [
        {id: uniqueId('todo-'), label: 'todo 1', done: true},
        {id: uniqueId('todo-'), label: 'todo 2', done: false},
        {id: uniqueId('todo-'), label: 'todo 3', done: true},
        {id: uniqueId('todo-'), label: 'todo 4', done: false},
      ]
    }
  },
  methods: {
    addTodo (todoLabel) {
      this.todoItmes.push({id: uniqueId('todo-'), label: todoLabel, done: false})
    },
    updateDoneStatus (todoId) {
      const whichUpdate = this.todoItmes.find((item) => item.id == todoId)
      whichUpdate.done = !whichUpdate.done
    },
    todoEdit (todoId, newLabel) {
      const item = this.todoItmes.find((item) => item.id === todoId)
      item.label = newLabel
    },
    todoDeleted (todoId) {
      console.log('todoDeleted id: ', todoId)
      const itemIdx = this.todoItmes.findIndex((item) => item.id === todoId)
      this.todoItmes.splice(itemIdx, 1)
    }
  },

  computed: {
    listSummary () {
      const finishItems = this.todoItmes.filter((item) => item.done).length
      return `${finishItems} out of ${this.todoItmes.length} items completed`
    }
  }
}
</script>

<style>
/* #app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */

/* 全局样式 */
.btn {
  padding: 0.8rem 1rem 0.7rem;
  border: 0.2rem solid #4d4d4d;
  cursor: pointer;
  text-transform: capitalize;
}

.btn__danger {
  color: #fff;
  background-color: #ca3c3c;
  border-color: #bd2130;
}

.btn__filter {
  border-color: lightgrey;
}

.btn__danger:focus {
  outline-color: #c82333;
}

.btn__primary {
  color: #fff;
  background-color: #000;
}

.btn-group {
  display: flex;
  justify-content: space-between;
}

.btn-group > * {
  flex: 1 1 auto;
}

.btn-group > * + * {
  margin-left: 0.8rem;
}

.label-wrapper {
  margin: 0;
  flex: 0 0 100%;
  text-align: center;
}

[class*="__lg"] {
  display: inline-block;
  width: 100%;
  font-size: 1.9rem;
}

[class*="__lg"]:not(:last-child) {
  margin-bottom: 1rem;
}

@media screen and (min-width: 620px) {
  [class*="__lg"] {
    font-size: 2.4rem;
  }
}

.visually-hidden {
  position: absolute;
  height: 1px;
  width: 1px;
  overflow: hidden;
  clip: rect(1px 1px 1px 1px);
  clip: rect(1px, 1px, 1px, 1px);
  clip-path: rect(1px, 1px, 1px, 1px);
  white-space: nowrap;
}

[class*="stack"] > * {
  margin-top: 0;
  margin-bottom: 0;
}

.stack-small > * + * {
  margin-top: 1.25rem;
}

.stack-large > * + * {
  margin-top: 2.5rem;
}

@media screen and (min-width: 550px) {
  .stack-small > * + * {
    margin-top: 1.4rem;
  }

  .stack-large > * + * {
    margin-top: 2.8rem;
  }
}

/* 全局样式结束 */
#app {
  background: #fff;
  margin: 2rem 0 4rem 0;
  padding: 1rem;
  padding-top: 0;
  position: relative;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 2.5rem 5rem 0 rgba(0, 0, 0, 0.1);
}

@media screen and (min-width: 550px) {
  #app {
    padding: 4rem;
  }
}

#app > * {
  max-width: 50rem;
  margin-left: auto;
  margin-right: auto;
}

#app > form {
  max-width: 100%;
}

#app h1 {
  display: block;
  min-width: 100%;
  width: 100%;
  text-align: center;
  margin: 0;
  margin-bottom: 1rem;
}
</style>
